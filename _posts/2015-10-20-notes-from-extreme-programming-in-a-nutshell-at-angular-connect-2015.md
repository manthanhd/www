---
id: 263
title: Notes from Extreme Programming in a nutshell at Angular Connect 2015
date: 2015-10-20T14:09:43+01:00
author: Manthan Dave
layout: post
guid: https://www.manthanhd.com/?p=263
permalink: /2015/10/20/notes-from-extreme-programming-in-a-nutshell-at-angular-connect-2015/
geo_latitude:
  - "51.5073509"
geo_longitude:
  - "-0.1277583"
geo_public:
  - "1"
categories:
  - Educational
tags:
  - Angular
  - Angular Connect
  - notes
---
13:30

Day one

Extreme programming in a nutshell. 

Extreme programming has practices that keep you safe. A bunch of practices that act as a safe net against failure. 

Of all the practices, going to talk about pairing, test first and continuous delivery. 

Starting with pair programming. Similar to Pacific Rim. 

<!--more-->

All production code is developed by more than one person. When pairing, design decisions are discussed between pairs. 

Two roles, navigator and worker. Switch pairs everyday to make sure that everybody is exposed to the code. Everyone is responsible for how the code works. 

Make best use of the team expertise. Use everybody's skills to the fullest and enable cross functional skills share. Allows sanity checking of ideas, avoiding rabbit holes. 

Taking pair programming to more extreme level. Mob programming. Taking the driver and navigator idea to a team. Whole team sits around a screen with one person with the keyboard. 

Driver types while team navigates. Run a timer to switch roles in and out. Kinda like coding dojo but do it in an open workspace. 3 or 4 people works the best. Fits nicely into size of ideal agile teams. 

This means that if one of the person has to go to the meeting the work continues. 

Use spikes. Spikes are done solo. Mob might split up and then do spike of their own. Each spike is an investigation. Prove out an architecture or a design idea and then bring it in and explain it to the team. Code produced is thrown away. This is done to maintain code quality and not use something that was developed in a rush. 

Don't use code reviews. The process causes a longer delay between creation and release. Also causes bottleneck. Pair / mob programming removes the need of this. 

Now talking about Test First. 

Gives confidence to deploy and refactor. Avoid manual mistakes. Keeps focus on what we are building now instead of other distractions on what it might be in future. 

Create acceptance test first. Helps focus implementation. Make sure we only build what we need. Helps us with continuous delivery. 

Now talking about continuous delivery. 

Deploy several times a day. From implementation to live in a couple of hours. Keep development pipeline short.

Use extensive monitoring to help track impact of releases. Measure various metrics, from hard metrics like CPU monitoring to actual speed of responses. 

Always committing to master. Avoid problems by decouple releases from development. Code is always released but use feature toggles to turn new features on or off. Settings based on environments. 

Toggles don't stay for forever. Once it's production ready and proven, it gets taken off and it's permanently on. 

Use mutex for deployments instead of ci pipeline. Use deployment tokens like a teddy bear to allow teams to deploy to production. 

Now top 3 tips even if you're not in extreme programming 

1. Add monitoring. 
Use it even if you're not using continuous delivery. Even if code is not changed, the environment might change. Start with generic health check and then proceed to what you need. 

2. Test first bug fix. 
Diagnose the issue and the  reproduce the steps. Once done, write a test that reproduces the issue. 

3. Pair code reviews
Review code while pairing. Eliminate bottlenecks. 

Add practices from XP incrementally and see what works for you. 

Tech.unruly.co