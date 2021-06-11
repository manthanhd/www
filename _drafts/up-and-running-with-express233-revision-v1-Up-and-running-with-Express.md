---
id: 234
title: Up and running with Express
date: 2015-08-31T14:11:38+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2015/08/31/233-revision-v1/
permalink: /?p=234
---
For those who don't know, Express is a web server framework that runs on Node.js. Basically, if you want to write a web application in node.js, express is one of your options. I prefer it because of its simplicity.

The quickest way to write an express application is to use the express npm module. If you haven't got it, install it by executing:

<code>npm install -g express</code>

Once installed, go to your projects folder, where you normally keep your projects and then execute:

<code>express myawesomeproject</code>

You should see some output and hopefully no errors. This command creates a basic project structure under <code>myawesomeproject</code> directory with some files for you to quickly get started. Some notable files are <code>./bin/www</code>, <code>./app.js</code> and <code>./routes/index.js</code>.

Now, <code>cd</code> into the project folder. You'll see several files and folders in that directory. Don't worry about that now. Let's get the application server up and running. However, before we can do that, we'll have to install the project dependencies. See the file called package.json? It defines the project and all its dependencies. NPM, being a dependency management system will go through the list of dependencies defined here and will then fetch them for you. If those dependencies have any tertiary dependencies then it'll fetch them as well. To install the dependencies, execute:

<code>npm install</code>

If you have a working internet connection, then this should go smoothly. Now let's run the application. To do this, execute the following:

<code>npm start</code>

Open your favourite web browser and navigate to the following url:

<code>http://localhost:3000/</code>

You should see a page with Welcome to myawesomeproject as its heading.

Awesome work! You've just created your first express web application.&nbsp;

In the next tutorial we'll go through the project structure to see how it all fits together.