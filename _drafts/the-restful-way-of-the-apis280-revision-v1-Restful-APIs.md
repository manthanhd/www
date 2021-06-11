---
id: 282
title: Restful APIs
date: 2016-01-10T12:16:02+00:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2016/01/10/280-revision-v1/
permalink: /?p=282
---
<p style="text-align: justify;">So, it's been a while now since I have written a post reflecting my thoughts on a subject. It has been primarily like this since I've been busy doing what I always do - trying out new cool things and ideas.</p>
<p style="text-align: justify;">In almost every app, every SaaS platform or every web based backend system I write, the first thing I start with is an API. This API first attitude has helped me in many ways. First and foremost, it helps me think how the architecture must work to suffice the needs of the client. It also leads me to think about containers and how best to distribute load etc. Now, this might seem like an overkill when just trying out a new idea but it at least gives me a rough estimate of the scope of the project that I'm looking at. Besides, now a days, working with thick client frameworks like Angular JS almost demands that you have a strong backend with a well written collection of APIs.</p>
<p style="text-align: justify;">Writing restful APIs time and time again have helped me improve my understanding regarding the idea of "restful APIs" with every iteration and hence, I've come up with ways in which a RESTful API should be written. So, here we go.</p>
<p style="text-align: justify;"><strong>A URL path should contain singular nouns and nothing else. </strong>This comes straight out of the REST specification. However, there have been cases where I have been forced to use verbs but that has only been the case when I haven't been very clear about what that endpoint is doing. Before defining your endpoints, be very clear on what you are trying to achieve. For instance, once I was writing an endpoint for an application to process an incoming web hook. Hence, I named the endpoint "/process". While I actually did implement this, it kept bugging me throughout the implementation of the application. Finally, when that piece of functionality was coming to an end did I get crystal clear clarity regarding what that endpoint was doing. When a web hook gets triggered, things happen within the application. THAT thing is an Event! Without doubt, I quickly renamed the endpoint to "/event" and my mind was at peace.</p>