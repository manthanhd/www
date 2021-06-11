---
id: 440
title: 'Let&#8217;s design: Asynchronous service'
date: 2016-09-11T20:35:49+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2016/09/11/439-revision-v1/
permalink: /?p=440
---
Hey, here's a great idea. Let's design an asynchronous service! You might be thinking; "What's an Asynchronous Service? And why am I designing this mysterious thing?". Let's break that down.

Who said that services have to be synchronous and over HTTP? Think of HTTP as just one interface. Just one of many ways of interacting with a service. You could also completely ignore TCP. TCP is also one way of interfacing with a web service. You could be using UDP or whatever custom protocol you want to use as long as it communicates with your web service and it works for you. Many people have done this as well so you won't be the only one doing it. People have, in real world, ditched TCP because it was too slow and went straight down to UDP to increase communication speeds between their services.

Simply put, an asynchronous service is a service that responds asynchronously. No I'm not talking about the one that runs on JavaScript, although that kind of implementation behind the scenes can help. Traditionally you have a web service that accepts requests and responds to those requests. However, most of the times, the client that is talking to the web service has to wait until the target service responds back. That's a synchronous service. With asynchronous service, the client doesn't have to wait for a response. It sort of like a fire and forget kid of communication. The client wants something done so it sends a message somewhere and it is immediately released once the message is received. The server can then process that message whenever it pleases.

Most asynchronous architectures focus heavy on the message aspect of it. This means that it is almost critical that the message that was sent is not lost somewhere once it's receipt was acknowledged. Simply put, if the server said it is going to do something at some point, it should do it. To make sure that the message is not lost, servers use something like a message queue.