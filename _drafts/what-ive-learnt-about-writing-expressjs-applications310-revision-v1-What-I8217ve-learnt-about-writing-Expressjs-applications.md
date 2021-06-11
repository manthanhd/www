---
id: 318
title: 'What I&#8217;ve learnt about writing Expressjs applications'
date: 2016-02-07T09:31:16+00:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2016/02/07/310-revision-v1/
permalink: /?p=318
---
There's a lot of ways you could write an express application. If you searched for Express app generator, you'll probably find at least five in the first page of search results.

In my opinion, most of these are designed for hackathons - to get you up and running as fast as possible but also as crudely as possible. While they are good when you want to try something out quickly, most of that code will turn into a big pile of mess as your application grows.

I've been in this situation before. I get my application up and running as fast as possible and in order to get my first MVP done but then quickly realise that the auto generated code that was meant to accelerate my development is now slowing me down. To remedy this, this is now how I develop my node.js applications.<!--more-->

Just a quick note. I still use the <span class="lang:default decode:true crayon-inline ">'express-generator</span> ' module for all my express applications.

Every <span class="lang:default decode:true crayon-inline ">Route</span>  file should have its own function. Consider this:
<pre class="lang:js decode:true">// file: routes/IndexRoute.js 
function IndexRoute(express) {
     var router = express.Router();
     router.get('/', function(req, res) {
         return res.render('index');
     });
     return router;
}

module.exports = IndexRoute;</pre>
Now, implement this in <span class="lang:default decode:true crayon-inline ">app.js</span>  like so:
<pre class="lang:js decode:true ">// file: app.js 
var express = require('express'); 
... 
var indexRoute = require('./routes/IndexRoute')(express); 
... 
app.use('/', indexRoute);</pre>
I quite like this approach because it keeps code clean and avoids having to use <code>require</code> everywhere. Also, by just looking at the module you can figure out what it depends on. For instance, here in <span class="lang:default decode:true crayon-inline ">IndexRoute.js</span>  file (above) you can tell that it depends on express. For more complicated modules that require additional dependencies you can keep adding them in the module function declaration. For example:
<pre class="lang:js decode:true">function UserRoute(express, userModel, moment) { 
... 
}
module.exports = UserRoute;</pre>
Here the <span class="lang:default decode:true crayon-inline">UserRoute</span> clearly depends on express, <span class="lang:default decode:true crayon-inline">userModel</span> (mongoose model) and moment (npm moment.js).

Sometimes when developing cool and awesome stuff, it is easy to get carried away. For instance, in the case above, the <span class="lang:default decode:true crayon-inline ">UserRoute</span>  is doing too much. It's doing something with time using moment and also manipulating user in the database using <span class="lang:default decode:true crayon-inline ">userModel</span> . While it's borderline fine (with massive cringe), you might want to think about what the <span class="lang:default decode:true crayon-inline">UserRoute </span> should actually be doing.

In most cases, the way I group responsibilities is following. Everything suffixed with <span class="lang:default decode:true crayon-inline ">Route</span>  manages Expressjs route mappings. One level down is <span class="lang:default decode:true crayon-inline ">Controller</span>  which actually manages what's being done to that endpoint that has been mapped. Another level down is <span class="lang:default decode:true crayon-inline ">Service</span> . This layer is more intelligent than controller and most of your logic should go in here. Lastly, you have <span class="lang:default decode:true crayon-inline">Model</span> . In case of mongoose this is just a model definition but there's nothing to stop you from doing more complex data interactions here.

This kind of code separation really helps as it is highly cohesive which in turn makes it testable. Now you can test the service and inject all its dependencies directly using the function parameters. Also, when you change definition of a service, it will break all your tests and yes that is a good thing because those broken tests will tell you the size of your impact crater that the change has created. If you didn't have those tests in place, you might have never known and maybe spent hours debugging trying to find out what went wrong. I know this because I've been there!

One more advantage of this that I can see is that it will stop you from accidently redefining something in your code that you would've if you used require all the time. This is especially true with Mongoose model definitions. It doesn't like it when you define a model twice and that is really easy to do when you're using require. Using this method, you require your model once in app.js and then pass it down to every service that requires it. In some cases it could also make your code more efficient as you aren't redefining objects evey time something like a service call is made.

I'm still working on finding the best way to write applications. Every time I write a new application, I learn about a new problem which I then set out to solve. I hope you do too!