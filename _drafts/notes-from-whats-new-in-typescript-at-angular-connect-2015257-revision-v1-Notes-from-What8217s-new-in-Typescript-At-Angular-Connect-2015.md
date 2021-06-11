---
id: 258
title: 'Notes from What&#8217;s new in Typescript? At Angular Connect 2015'
date: 2015-10-20T12:03:00+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2015/10/20/257-revision-v1/
permalink: /?p=258
---
<strong>1100</strong>

Day One

What's new in Typescript? By Bill Ticehurst 

Goals is to make javascript development more productive and enjoyable. 

Hard to work in code bases with lack of types. Hard to debug. 

<!--more-->

Superset of javascript that compiles into plain javascript. Simple mapping. Makes it easy to debug, back and forth. 

ES 6 sounds futuristic. Once you get used to it, it's hard to go back. 

Static types allow better documentation which in turn gives better tooling. Allows jumping around points in code easier. 

Types are optional. Don't need to have types everywhere. Kinda like code coverage. Do what you need. 

Now showing demos. There's a plugin available for sublime which helps writing Typescript easier. Now urging audience to find bugs in presented demo code. Interfaces help debug code as it sets expectations for assignable types. 

Live stack trace. Bugs and trace disappears as you fix them. How cool is that? 

Debugging in this thing is amazing. Errors are helpful and intellisense is better as the code knows things! 

A variable can have more than one type. Like string or a string array. In this case, the Typescript expects common methods in both types to be valid. If specific methods needs to be used then need to cast or parse it. 

Generics! 

“this demo has blown my mind.”

Now showing a demo with angular.

Imports works dynamically with node modules. It just works! 

Urging developers to start distributing Typescript files along with their modules. 

Decorators are able to interfere with the declaration of an object. Logger decorator injects function before execution of the target function. 

Reflection API. Decorators don't modify the object but adds metadata using the reflection API. 

Reflect.getmetadata(“annotations”, varname) 

Embed html code directly in javascript in jsx files. Typescript allows autocompletion in jsx files for both, html and Typescript / javascript. Coolest thing I've ever seen. 

Powerful project wide find references tooling + variable name refactoring. “is this available in all Typescript plugins?”

ES 6 async and await looks incredibly powerful way to control async functions execution flow. 

await Promise.all(arrayOfPromises) 

Waits for all promises to be fulfilled before executing any of the remaining code.