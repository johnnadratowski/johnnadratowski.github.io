---
layout: post
title:  "What if, one day"
date:   2016-08-05 12:56:36 -0400
tags: software
---

*I understand that some readers may be offended by the content of this article.
While this article is trying to prove a point (however hamfisted and sophomoric 
the execution may be), it is written in jest with the aim of entertaining the reader.*

*Please try to keep in mind that this article is written out of fun and 
is in no way intended to be offensive to anyone or any group of people.*

*However, if you do have negative feedback, you can send it [here](mailto:HolyZionsHelpForever@wbcstuff.com).*

-----------------------------------------------
{: .hr }

This post is about four simple words that have been the downfall of software projects,
technology companies, and everything in between.  When I hear these four little words it’s like
a rabid mule reared up and kicked me right in the baby makers.  I cannot spew enough vitriol to
be able to possibly describe the amount of anguish I have for them.  They have been a waste of
countless hours of my time and the time of some of my best friends, at the detriment of our
very lives.

Those words are *“What if, one day...”*.

Lets see if you’ve ever been in conversations like these:

* Of course we should make the data model dynamic! What if, one day, the user wants to add their own custom fields?
* Well, we think it’s a good use of our time to add a configurable workflow to this application! What
if, one day, the user wants to customize their workflow?
* Postgres will never hold up! What if, one day, we’re processing like, 10 judobytes of data per second?
* You fool! The left-pad module should absolutely be it’s own microservice! What if, one day, Twitter is
trending on left-padding and every single user in our system wants to left-pad everything that could
possibly be left-padded so they start left-padding their left-pads!

![](http://does.not.exist.com/look-somewhere-else){: .center-image }
Insert extremely dated XHibit meme about left-padding your left-pads here. Screw you all, I loved that meme.
{: .image-caption }

If you have, my friend, you deserve all of our deepest sympathies.  But you won’t get them here. Oh no no no.
Unless you wound up and threw a haymaker straight at that human filth’s pie-hole, I have no sympathy for you.
You might have even *gasp* thought it was a good idea at the time! You poor bastard.

Lets try a little thought experiment to demonstrate the fallacy in this thinking.  Imagine you were tasked to build
a horse cart.  You know, the ones the Amish use?  Good, we’re on the same page.

So you start designing your horse cart, and of course it’s going to be super awesome, because you’re super awesome,
and everyone should know that.  So, you give the project some hyperbolic code name, PROJECT YOUR-BALLS-ARE-GUNNA-FLY-OFF, 
and you start building your horse cart.

A month into the project, everything is going swimmingly.  You got a seat for the driver, reigns for the horses, hell,
you even put a little canopy on the friggin' thing because you thought the Amish dude’s children might not have access
to proper sunscreen and could get sun burnt.  You have no clue what Amish people do, you should read up on it, you’re
ignorant to the point of bigotry. But the foresight! You really are a god among men.

Then one day you think, *”Well, what if, one day, these Amish people get out of the dark ages and want to put a motor on this thing”*.
The next day you’re thinking about the poor children again, and you think *”What if, one day, this thing flips over! I need to add
a roll cage to this mofo”*.  Yet again the next day you think *”I still haven’t read up on these Amish people, what if they are really
into off-roading with these suckers? I should really throw some shocks and all-terrain wheels on here”*.

That last point is admittedly kind of badass, and you could probably persuade me in that design meeting.
{: .image-caption .center-image }

And, before you know it, you just created the most jacked-up car that ever did exist. Kudos, you did it! You might even
get the illest project award at your companies next holiday party.  Good for you.


![Google image result for the most jacked-up car ever](http://cbsnews3.cbsistatic.com/hub/i/r/2011/09/01/ac446744-a644-11e2-a3f0-029118418759/resize/620x465/87efc6f4f9e0b813b7ace0cac55ef383/photo_1_540x386.JPG){: .center-image }
A Google image result for the most jacked-up car ever
{: .image-caption }



## Get to the point already, you rambling fool

The point is, when we code for known unknowns, it is only ever luck if you actually end up building the right thing.

Software requirements change all of the time, and very, very few projects know 90% of their requirements up front. Even when
you think you know the requirements, it’s most like that they will change anyway.  That’s because software is made
for people, and [small groups of people are very poor at judging what a large group of people will think.](https://www.youtube.com/watch?v=uf2fTtWSnOA)

I’m not saying that you shouldn't defensively code against what you know is going to happen.  If you know that 
your load is going to increase significantly in a short amount of time, by all means, code for that use case.
However, even if you don’t make the most efficient implementation possible, it’s more likely that you’ll be able
to buy yourself enough time by throwing hardware at the problem.  In the end, when you do your refactor, it will
be a lot easier because of the simplicity of the original implementation.

These things really do kill projects, and even whole companies.  When you’re in a competitive atmosphere, it’s not
the companies that work harder, but smarter.  It simply does not make sense to spend a significant amount of time
coding a feature that you don’t know will be used.  Most of the time, you end up with way more code than you need,
which is a liability. Compound that with the fact that many engineers don’t properly document their 
implementations, and you have a hidden beast that no one fully understands making all of our work moving forward
that much harder.

So, the next time you hear the words “What if, one day”...

**RUN**

