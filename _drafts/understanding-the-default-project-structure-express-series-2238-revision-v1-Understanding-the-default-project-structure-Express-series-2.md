---
id: 242
title: Understanding the default project structure (Express series 2)
date: 2015-09-01T21:50:12+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2015/09/01/238-revision-v1/
permalink: /?p=242
---
Now that we have a basic express web app created, lets go through the project structure.

<strong>package.json</strong>
In the last post we briefly went through the purpose of package.json. Now its time to go through it in bit more detail. As the extension <code>.json</code> indicates, data in this file is in <code>json</code> format. Yours should look something like this:<!--more-->

<code>{
"name": "myawesomeproject",
"version": "0.0.0",
"private": true,
"scripts": {
"start": "node ./bin/www"
},
"dependencies": {
"body-parser": "~1.12.0",
"cookie-parser": "~1.3.4",
"debug": "~2.1.1",
"express": "~4.12.2",
"jade": "~1.9.2",
"morgan": "~1.5.1",
"serve-favicon": "~2.2.0"
}</code>

The name and version fields indicate the project name and version respectively. The field private indicates that this is a private npm project. So, in case you were developing a npm module, this field will determine whether or not to allow you to publicly publish your project. The scripts field contains commands and the respective files that should be executed when the command is called. The two most popular ones here are start and test. As you can see, the express tool automatically generates the start script command and maps it to ./bin/www.

The last thing here is the dependencies field. This contains all the dependencies of your project. The tilda sign (<code>~</code>) prefixed in the version means that if a newer bug fix version is found then it'll update it to that automatically. For instance, in our case, the current version of express module is 4.12.2. In case a newer version 4.12.3 is released, the tilda sign means that npm will fetch that version. However, if express 4.13 is released, npm won't update to that. On the contrary, if the hat sign (<code>^</code>) was used, npm will update to 4.13 but will hold off for version 5.0.

<strong>app.js</strong>
This file is the central point for configuring the express web server. It defines several things like the view engine, logging levels etc and loads all middleware components (more on that in later posts). You'll be visiting this file more often than you think once you start developing express web applications.

If you scroll down, you'll see that this file also loads routes. This is where the server routes are defined. In this case, the route file <code>index.js</code> is loaded in and all its routes are mapped from the root route (indicated by <code>/</code>). In the next line it also loads another route file called <code>users.js</code>, routes of which are mapped from <code>/users</code>.

<strong>routes</strong>
This directory contains files that define different routes of the web application. You can define routes in multiple files here and then reference them in the <code>app.js</code> in parent folder. This helps to keep things separate from one another to avoid clutter. Although JavaScript has no concept of a "class", you can define various objects here and then re-use them across your various routes. We'll go over that in later posts.

<strong>views</strong>
This directory contains all your templates. Since the default templating engine is Jade, you'll find files with <code>.jade</code> extension here. You can use one of the other options that express tool provides like ejs or handlebars or you can choose a completely different templating engine as well. Whatever you choose will have to be configured in the <code>app.js</code> file. The templates that have been defined here can be changed on the fly, meaning that changing the template while the application is running will have immediate effect. In contrast to this, if you change one of the javascript files, you'll have to restart the whole application. Check out Jade templating engine here <a href="http://jade-lang.com/">http://jade-lang.com/</a>

<strong>public</strong>
As the name suggests, this directory contains all the public assets. Whatever you place in here can be accessed without authentication as these are the assets that pages normally require to render. Any files here can be referenced from root level. For instance, a stylesheet named <code>style.css</code> located in stylesheets folder in public directory can be accessed by <code>/stylesheets/style.css</code> from the web server. You can create as many sub directories as you want without making any configuration changes in <code>app.js</code>.

Excellent! Now you know what different parts of an express web app does. In the next post we'll create our first RESTful web service using Express. Excited?