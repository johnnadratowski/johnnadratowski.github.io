---
date: 2016-08-05T12:56:36Z
tags: 
    - golang 
    - tls 
    - ssl 
    - networking 
    - bolt 
    - neo4j
title: Client TLS Connections in Golang
---

TLS has always been a bit of a mystery to me.  I understand how it works from a high
level, but when it comes to the internal workings of it I have no clue.  Hence, when an 
[issue][graphene-issue] for my [Golang Neo4j Bolt Driver][golang-neo4j-bolt-driver] 
project turned out to be related to lack of TLS support, 
I decided it would be a great subject for a post.

The issue stated that the connection wasn’t working to [GrapheneDB][graphenedb]
and that it was using a non-TLS connection.  TLS support wasn’t yet implemented for
the driver, which was documented in the README.

I started by assuming that it was an issue with how the driver
was interacting with their service.  After digging in I couldn’t find any
reason that would be. I decided to see if adding in the simplest TLS
support initiated a connection.

I had no idea what Golang nor Neo4j supported for TLS connections, so I 
decided to check out a reference implementation in [neo4j-python-driver][neo4j-python-driver]
made by [Nigel Small][nigel-small].  He has been a great help in clarifying some questions I had
when I was creating the driver (Thanks Nigel!).

The python driver specified protocol [PROTOCOL_SSLv23][neo4j-tls-protocol] 
which supports [TLSv1->TLSv1.2][python-socket-creation]. So far, so good.

The [Golang Docs for TLS][golang-tls] docs are a little sparse, so I tried to find some
information elsewhere that had some good examples.  After finding [this gist][tls-golang-gist]
I had enough for the simplest (read: insecure) TLS client implementation. 

{{< highlight go >}}
if c.useTLS {
    conn, err = tls.Dial("tcp", url.Host, &tls.Config{
        MinVersion: tls.VersionTLS10,
        MaxVersion: tls.VersionTLS12,
        // InsecureSkipVerify means to accept whatever cert you get from the server
        // Subject to man-in-the-middle attacks. Golang docs say only for testing.
        InsecureSkipVerify: true, 
    })
    if err != nil {
        return nil, nil, "", "", err
    }
}
{{< / highlight >}}

And... viola! We have a properly established connection.

What next? Well, obviously this isn’t secure enough. I set out to familiarize myself
with the Golang TLS configuration and the way these TLS certificates and keys are used.

I ended up finding 
[Creating Your Own SSL Certificate Authority (and Dumping Self Signed Certs)][ssl-certs]
which was a major help.  I was able to generate the keys necessary to test this using
the following steps:

{{< highlight bash >}}
# Create the root private key
openssl genrsa -out rootCA.key 2048
# Create the root certificate pem file
openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 1024 -out rootCA.pem
# Create the Neo4j private key
openssl genrsa -out neo4j.key 2048
# Create the certificate signing request. (Make sure to set the Common Name to the server domain)
openssl req -new -key neo4j.key -out neo4j.csr
# Create the Neo4j certificate
openssl x509 -req -in device.csr -CA rootCA.pem -CAkey rootCA.key -CAcreateserial -out neo4j.cert -days 500 -sha256
# Copy certificate + key to neo4j certificates folder
cp ./neo4j.key ./neo4j.cert /path/to/neo4j/certificates/
{{< / highlight >}}

I updated the go code to have the following implementation, based on another [gist][ca-cert-gist]:

{{< highlight go >}}
if c.useTLS {
    config := &tls.Config{
        MinVersion: tls.VersionTLS10,
        MaxVersion: tls.VersionTLS12,
    }

    if c.caCertFile != "" {
        caCert, err := ioutil.ReadFile(c.caCertFile)
        if err != nil {
            return nil, nil, "", "", err
        }
        caCertPool := x509.NewCertPool()
        caCertPool.AppendCertsFromPEM(caCert)
        config.RootCAs = caCertPool
    }

    conn, err = tls.Dial("tcp", url.Host, config)
    if err != nil {
        return nil, nil, "", "", err
    }
}
{{< / highlight >}}

After updating the config for my local Neo to require SSL and running my tests, all 
passed successfully.  I then attempted to run them against GrapheneDB using no custom
ca certificate, which was also successful.

In summary, even though I think the Golang docs for this library are lacking, there
were enough resources available on the web to make this fairly trivial.  The library
should now support TLS both using a CA and a self-signed keypair.


[golang-neo4j-bolt-driver]: http://jekyllrb.com/docs/home
[graphene-issue]: https://github.com/johnnadratowski/golang-neo4j-bolt-driver/issues/5
[graphenedb]:   http://www.graphenedb.com/
[neo4j-python-driver]: https://github.com/neo4j/neo4j-python-driver
[nigel-small]: https://github.com/nigelsmall
[neo4j-tls-protocol]: https://github.com/neo4j/neo4j-python-driver/blob/1.1/neo4j/v1/session.py#L141
[python-socket-creation]: https://docs.python.org/2/library/ssl.html#socket-creation
[golang-tls]: https://golang.org/pkg/crypto/tls/
[tls-golang-gist]: https://gist.github.com/denji/12b3a568f092ab951456#tls-transport-layer-security--client
[ssl-certs]: https://datacenteroverlords.com/2012/03/01/creating-your-own-ssl-certificate-authority/
[ca-cert-gist]: https://gist.github.com/michaljemala/d6f4e01c4834bf47a9c4#file-tls-client-go-L27

