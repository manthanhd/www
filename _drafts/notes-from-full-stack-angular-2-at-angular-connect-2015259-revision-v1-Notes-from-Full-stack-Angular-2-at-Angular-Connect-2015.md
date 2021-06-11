---
id: 260
title: Notes from Full stack Angular 2 at Angular Connect 2015
date: 2015-10-20T13:06:20+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2015/10/20/259-revision-v1/
permalink: /?p=260
---
11:30

Day one 

Full stack angular 2

Why full stack? 

Why angular? 

Best tool for the job doesn't always win. Sometimes it's political driven. 

Communication problems. Context switching adds unnecessary overhead. 

Same things needs to be implemented many times over and over again. 

Committees, contention, context switching, code duplication. Problems in today's world. Time wasted is significant. 

<!--more-->

Challenges to use same technology across whole stack. Javascript gets bad rep sometimes as it used to be used only in the web browser. 

ES6 and ES7 plus angular 2 will make it more and more prominent for use in full stack. 

Lots of features makes it powerful. Dependency injection is one of the examples. 

Complex flows like communicating from browser to server to database still results in code duplication, even with javascript. 

Use dependency injection, one way to eliminate dom references, making it faster. 

Still DI is not ideal as DI adds complexity overhead. 

Demo is showing DI in action, switching implementation quickly by only modifying one line of code. 

@injectable annotation makes it easier to use DI. 

Now talking about Angular 2 Universal. 

Write client side app and just install the server side plugin to allow easy access via model. Basically converting a client side application to do server rendering. 

Example showing conversion of client side application into server side. 

Importing client side main angular app file on server and then setting it as model object to pass on to render method for rendering index view. 

Using ng2engine module from angular2-universal-preview module. Setting instance of that as app engine for all .ng2.html files and then binding ng2.engine as view engine in express app. 

Now reloading the server. Now the view is rendered on the server side! 

Working towards supporting more server based environments. 

Full stack angular 2 transforms full stack Web development. 

bit.ly/ng2stack

Fullstackangular2.com

@jeffwhelpley @gdi2290