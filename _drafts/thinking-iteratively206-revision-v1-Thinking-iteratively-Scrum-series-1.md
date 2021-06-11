---
id: 225
title: Thinking iteratively (Scrum series 1)
date: 2015-08-27T17:03:56+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2015/08/27/206-revision-v1/
permalink: /?p=225
---
Most of us think linearly, that is step by step, from start to finish. Think about making something physical, like a wooden table. Your mind quickly starts creating steps leading to that goal. Something like this feels familiar?
<ol>
	<li>Buy lots of raw wood.</li>
	<li>Buy sandpaper.</li>
	<li>Buy varnish.</li>
	<li>Make four legs.</li>
	<li>Make table top.</li>
	<li>Apply sandpaper.</li>
	<li>Join it all up.</li>
	<li>Apply varnish.</li>
</ol>
<!--more-->

Sorry, I'm no expert in the art of table making but most of you would have a similar list of actions. This is linear, meaning that the table cannot be used at all unless all the steps are complete. If you don't have a dining table at all in your house, then you'll probably have to sit somewhere else to eat for a couple of days (depending on how good you are at making a wooden table).

Now think about writing software, say a website to help out your friend sell hats for cats online. Automatically you'll start thinking:
<ol>
	<li>Design frontend.</li>
	<li>Develop backend.</li>
	<li>Implement frontend.</li>
	<li>Integrate payment systems.</li>
	<li>Integrate email notification service.</li>
	<li>Test.</li>
	<li>Deploy.</li>
	<li>Handover.</li>
</ol>
There's probably a lot of intermediary steps but this is the gist of it all. Again, if you look through carefully, your friend does not get her website unless all of the steps are complete.

Iterative development only means that instead of doing everything in linear sequential steps, you are doing it in cycles. Delivery is key here meaning that you should aim to deliver at least once by the end of each cycle but mature teams are able to deliver the product more than twice each cycle. Thinking in line with the Scrum methodology, each cycle (or sprint) can be one week or up to one month. In our case, if we choose that one cycle is one week, here's what it might look like:
<ol>
	<li>Cycle 1:
<ol>
	<li>Design basic frontend.</li>
	<li>Implement mock backend.</li>
	<li>Implement basic frontend.</li>
	<li>Integrate mock payment system.</li>
	<li>Integrate mock email notification service.</li>
	<li>Deploy and deliver.</li>
</ol>
</li>
	<li>Cycle 2:
<ol>
	<li>Improve frontend design.</li>
	<li>Implement basic backend functionality still mocking rest.</li>
	<li>Implement improved frontend design.</li>
	<li>Integrate mock payment system.</li>
	<li>Integrate mock email notification service.</li>
	<li>Deploy and deliver.</li>
</ol>
</li>
	<li>Cycle 3:
<ol>
	<li>Implement more backend functionality.</li>
	<li>Revisit frontend designs.</li>
	<li>Implement frontend design changes.</li>
	<li>Integrate basic payment system.</li>
	<li>Integrate mock email notification service.</li>
	<li>Deploy and deliver.</li>
</ol>
</li>
	<li>Cycle 4:
<ol>
	<li>Implement more backend functionality.</li>
	<li>Revisit frontend designs.</li>
	<li>Implement frontend design changes.</li>
	<li>Integrate basic email notification service.</li>
	<li>Deploy and deliver.</li>
</ol>
</li>
	<li>Cycle 5:
<ol>
	<li>Implement more backend functionality.</li>
	<li>Integrate advanced payment system.</li>
	<li>Deploy and deliver.</li>
</ol>
</li>
	<li>Cycle 6:
<ol>
	<li>Implement more backend functionality.</li>
	<li>Revisit frontend designs.</li>
	<li>Implement frontend design changes.</li>
	<li>Integrate advanced email notification service.</li>
	<li>Deploy and deliver.</li>
</ol>
</li>
</ol>
Here, you are deploying and delivering the product to your friend every week so that she can inspect the whole flow. This also allows you to receive feedback regarding the entire product every week which is not possible in the linear way of doing things (as your product is not ready for your friend to look at until the end). Also you don't have to come up with all the cycles at once. You basically have to have the current cycle plus at least next two cycles within your sights.

Mocking things is very important in this flow as it helps your friend to "feel" how the website will look at the end. The mocks allow her to go through the checkout flow as if it was actually there. It might seem like a waste of time as you are developing something that you know will get replaced at the end anyway, it still has tremendous value as there is nothing worse than the client changing her mind and wanting to go in a completely different direction towards the end of the project. The mocks allow the client to focus as they get to see the product as it is being developed while also giving you continuous feedback.

While this was a simple example, it gets quite a bit complicated when new projects are undertaken in large scale organisations as there can be lack of clarity before a project is started. I'll do another post on the Scrum Framework for this sometime in near future but this post provides a base that can help you understand Scrum as it is also about iterative software development.

Again, if you observe the technological advancements that humans has achieved so far, you'll notice the iterative approach at large scale. We didn't aspire to build shiny cars from day one. We first started with the wheel, advanced to wheel barrow, horse cart, trains and then finally a built car.

Hope you enjoyed this article. I'll appreciate it very much if you could leave your thoughts in the comments below.