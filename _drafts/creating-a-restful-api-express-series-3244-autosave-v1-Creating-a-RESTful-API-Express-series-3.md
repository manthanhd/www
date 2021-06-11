---
id: 249
title: Creating a RESTful API (Express series 3)
date: 2016-01-10T14:04:14+00:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2015/09/02/244-autosave-v1/
permalink: /?p=249
---
So in the last post we went over the project structure. This time, we'll actually modify some of the code to create our own RESTful web service.

If you don't know what a RESTful web service is, you should check out the <a href="https://en.wikipedia.org/wiki/Representational_state_transfer" target="_blank">wikipedia</a> entry for it.

As discussed in the previous post, all of our routes are defined in files located in the routes folder which are then referenced and collated in the app.js file. So, to create our own RESTful API, we need to do something in this area.

Lets checkout the <code>index.js</code> file and see whats in it.<!--more-->
<pre class="lang:js decode:true ">var express = require('express');
var router = express.Router();

/* GET home page. */
router.get('/', function(req, res, next) {
res.render('index', { title: 'Express' });
});

module.exports = router;</pre>
This is the typical structure of any nodejs module. Top section is to declare dependencies and instantiate objects. Middle part is to actually do things. And the final bottom part is to clean up and export the module. This file says that when the root (<code>/</code>) is accessed by a HTTP GET method (indicated by <code>router.get</code>), call a function. This function is defined as <code>function(req, res, next) {...}</code>. The three variables, <code>req</code>, <code>res</code> and <code>next</code> are function parameters. I won't go into the details regarding basic JavaScript in any of these tutorials. If you have any questions, check out some of many JavaScript resources available on the web.

These parameters <code>req</code>, <code>res</code> and <code>next</code> are the request object, the response object and the function call to the next module in the middle ware stack respectively. The line <code>res.render('index', { title: 'Express' });</code> is using the render function of the response object, asking express to render the template <code>'index'</code> with parameters specified by the JSON object <code>{ title: 'Express' }</code>. Since Express knows that we are using Jade templating engine (defined in <code>app.js</code>), it will go to the views folder and find the file named <code>index.jade</code> and will render it.

Right, so now that's out of the way, lets go and create our own service. For the purpose of this tutorial, we'll create a health check web service. When it gets called, it will return <code>OK</code> status indicating that the server health is ok. Since this will require a GET request to be invoked, we'll be using router.get and hence the first line of the code will be very similar to the already existing one.
<pre class="lang:js decode:true ">router.get('/', function(req, res, next) {
// My health check API
});</pre>
<code></code>Next, we need to change two things. One is the URL reference. Currently, its set to <code>'/'</code>. Because this has already been defined, lets modify it to our own path. This can be anything you want but for the sake of simplicity, lets make it <code>/healthcheck</code>. Your code should now look like this:
<pre class="lang:js decode:true ">router.get('/healthcheck', function(req, res, next) {
// My health check API
});</pre>
<code></code>The code doesn't do anything yet. Lets change that. Remove the comment <code><strong>// My health check API</strong></code> and put the following logic in it.
<pre class="lang:js decode:true ">var statusString = 'OK';
return res.send(statusString);</pre>
<code></code>After your change, the file <code>index.js</code> should look like this:
<pre class="lang:js decode:true ">var express = require('express');
var router = express.Router();

/* GET home page. */
router.get('/', function(req, res, next) {
res.render('index', { title: 'Express' });
});

router.get('/healthcheck', function(req, res, next) {
var statusString = 'OK';
return res.send(statusString);
});

module.exports = router;</pre>
Awesome. Now go back to your command line and run <code>npm start</code> command to start your server. Once started, go to <code>http://localhost:3000/healthcheck</code> on your web browser. You should see the following output:

<code>OK</code>

Now this isn't very clean as its returning plain text instead of proper JSON response. Shut down the server and go back to your IDE. Modify the code in the <code>/healthcheck</code> router in <code>index.js</code> to be the following:
<pre class="lang:js decode:true ">var statusObject = {status: 'OK'};
return res.send(statusObject);</pre>
<code></code>Start the server and navigate back to <code>http://localhost:3000/healthcheck</code> in your browser. You should see a JSON response this time:
<pre class="lang:default decode:true ">{

"status": "OK"

}</pre>
Great work! You've just written your first RESTful web service in Node.js using Express! In the next post we'll take this to a next level and migrate what we've written to a separate file to reduce clutter and make the code a bit cleaner. Enjoy!