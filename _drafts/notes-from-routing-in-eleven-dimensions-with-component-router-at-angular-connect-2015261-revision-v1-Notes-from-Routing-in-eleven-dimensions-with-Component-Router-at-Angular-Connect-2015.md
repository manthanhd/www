---
id: 262
title: Notes from Routing in eleven dimensions with Component Router at Angular Connect 2015
date: 2015-10-20T13:12:48+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2015/10/20/261-revision-v1/
permalink: /?p=262
---
12:00

Day One. 

Routing in eleven dimensions with component router by Brian Ford 

Route configuration. Mapping url to component. 

Route configuration meta data decorator helps to do this in angular 2. Use path parameters style (same as express) to define variables in routes when mapping. 

Inject route paramo service then in the target component to give access to those route params.

Use square bracket router-link (property binding) directive to provide links between routers. 

<!--more-->

From the RouteConfig, one can also link the routes by their id field (as attribute). If the route url has parameters then a map with parameters must be passed as second attribute to the property binding. 

This new Link DSL is robust and survives refractors better than URLs. Also provides a safety net in case one of the parameters aren't met. 

Child routes allows embedding routes within routes. Basically nested routes. 

To make this work, add route config with router outlet directive in the template. Denote if that route has child route by suffixing url with /… 

URLs to child routes are relative to the url of the parent route. They have their own parameters so don't need to worry about collisions. Could use id within parent route it the prefix of ./ notation. 

Now talking about auxiliary routes. Each component can have zero or more auxiliary routes. 

URLs are
/parent(parent-aux)/child(child-aux) 

A modal is just an auxiliary routes. It can be thought of like tree of trees.