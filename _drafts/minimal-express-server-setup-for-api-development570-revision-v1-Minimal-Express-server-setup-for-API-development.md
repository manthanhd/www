---
id: 618
title: Minimal Express server setup for API development
date: 2017-04-22T20:59:17+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2017/04/22/570-revision-v1/
permalink: /?p=618
---
Initialise npm with defaults.
<pre class="lang:sh decode:true">npm init</pre>
Create your main <span class="lang:default decode:true crayon-inline ">index.js</span> entrypoint.
<pre class="lang:sh decode:true">touch index.js</pre>
Install <span class="lang:default decode:true crayon-inline">express</span>, <span class="lang:default decode:true crayon-inline">body-parser</span>, <span class="lang:default decode:true crayon-inline">morgan</span> and <span class="lang:default decode:true crayon-inline">winston</span> packages.
<pre class="lang:sh decode:true">npm install --save express body-parser morgan winston</pre>
Make your <span class="lang:default decode:true crayon-inline">index.js</span> look like this.
<pre class="lang:js decode:true" title="index.js">const express = require("express");
const app = express();

global.port = process.env.PORT || 3000;
global.logger = require("winston");
logger.level = process.env.LOG_LEVEL || "debug";

const bodyParser = require("body-parser");
app.use(bodyParser.json());

const morgan = require("morgan");
app.use(morgan('combined'));

app.get("/", function(req, res) {
    return res.send({message: "hello"});
});

app.listen(port, function(err) {
    if(err) {
        logger.error(`Failed to listen on port ${port}.`);
        return process.exit(1);
    }

    logger.info(`Listening on port ${port}.`);
});</pre>
This is probably one of the most light weight node.js configuration that I have ever written for building simple REST web services.

In my opinion, this is a good starting point as it makes minimal assumptions about what you might need, letting you add whatever you need minimally on top.