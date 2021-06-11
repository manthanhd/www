---
id: 269
title: Notes from ngAnimate 2.0 at Angular Connect 2015
date: 2015-10-22T12:05:16+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2015/10/22/268-revision-v1/
permalink: /?p=269
---
Day two 

14:30

ngAnimate 2.0 by Robert Masserie 

No longer css. Css doesn't allow complex logic. Using domain specific language (DSL). 

Json file dictates chaining and choreography. Json is limited as it doesn't alow dynamic logic. Represents static data. 

Now using full DSL with full programmatic API. 

<!--more-->

ngAnimate 2.0 is a proof of concept as of now. 

Use of animation factory to create animations. Still allows one to define static animations but one can also use the programmatic API to include complex logic in animation flows. Seamlessly switch between parallel and sequential animations. 

Stagger animations by specifying key frames at various points in progress. For instance, apply colour change at 25%, and scaling at, 75%.

It comes with better event integration. For instance trigger a modal animation from the point of click on the button. This is possible using the programmatic API. 

Now talking about Material Design. 

Pretty difficult to do using angular 1 as some of the animations originated from the point of click. 

Now, listen on click events and then run the ngAnimate functions to handle the animations programmatically. 

Tab animations are tricky because it animates the tabs themselves, the ink bar that transitions left or right and the actual content below. This is implemented in angular 2 by setting the tab index and then programmatically loading the rest or the animations I. E. Left or right based on what the tab index is.

Now talking about what ngAnimate will be able to. 

Rich unit testing, performance tuning, migration and adaptive styling look like interesting goals. 

Animation asserts help test if an animation sequence has happened or not in unit test.

Addressing performance by no longer doing reflows. Also pre calculating delays to provide more efficient animations. 

Considering an upgrade path from ngAnimate 1 to 2.

Ability to control animations at any stage. Pause, fast forward or reverse. Showed a demo with a slider controlling full animation sequence of a mix of asynchronous and synchronous animation flows.