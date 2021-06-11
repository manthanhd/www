---
id: 468
title: Gulpfile.js for vanilla AngularJS projects
date: 2016-09-18T20:36:01+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2016/09/18/306-autosave-v1/
permalink: /?p=468
---
Since I've learnt Angular, most of my front-end web applications are written in it. Its brilliant. Everything works fine, up to a point where your application has grown 10 times the size it was since you started it. At some point you'd want something to accelerate your delivery. That's when gulp comes in.

Gulp is kinda like a build tool but for javascript based applications. It runs on Node.js and is available on npm. All you need to do is:

<pre class="lang:sh decode:true ">npm install -g gulp</pre>

or if you are using a unix based application:

<pre class="lang:sh decode:true ">sudo npm install -g gulp</pre>

That gets you access to the gulp command line. Once you have gulp installed, you need to something that tells it what to do with your project. Unlike many other build tools, gulp favours code over configuration. This means that rather than writing XML or YAML files that define the tasks, you write code. This is what makes gulp unique.

So, here's what I have. Below is my <code>gulpfile.js</code> that I use in most of my AngularJS projects:<!--more-->

<pre class="lang:js decode:true ">/**
* Created by manthanhd on 04/01/2016.
*/
var gulp = require('gulp'),
gutil = require('gulp-util'),
sourcemaps = require('gulp-sourcemaps'),
concat = require('gulp-concat'),
ngAnnotate = require('gulp-ng-annotate'),
watch = require('gulp-watch'),
uglify = require('gulp-uglify');

gulp.task('default', ['build-js']);

gulp.task('build-js', function() {
return gulp.src(['public/app/**/*.js'])
.pipe(sourcemaps.init())
.pipe(concat('bundle.min.js'))
.pipe(ngAnnotate())
//only uglify if gulp is ran with '--type production'
.pipe(gutil.env.type === 'production' ? uglify() : gutil.noop())
.pipe(gutil.env.type === 'production' ? gutil.noop() : sourcemaps.write())
.pipe(gulp.dest('public/dist'));
});

gulp.task('watch', function() {
gulp.watch('public/app/**/*.js', ['build-js']);
});</pre>

Most of my AngularJS code resides in <code>public/app</code> directory and hence you'll see that in multiple places in that file.

A quick glance should tell you that this minifies all of my AngularJS code and combines it all in one big file called <code>bundle.min.js</code> which is then placed in <code>public/dist</code> folder. My HTML page is configured to only reference <code>bundle.min.js</code>.

When executed normally, it doesn't minify the code because it assumes that the javascript code is being used for development. However, when I run <code>gulp --type production</code> it minifies the code and doesn't write sourcemaps.

You can install all of this by running:

npm install --save-dev gulp gulp-concat gulp-ng-annotate gulp-sourcemaps gulp-uglify gulp-util gulp-watch

Oh and here's my <code>package.json</code>:

<pre class="lang:default decode:true "> {
"name": "my-sample-angular-project",
"version": "0.0.0",
"private": true,
"scripts": {
"start": "node ./bin/www"
},
"devDependencies": {
"gulp": "^3.9.0",
"gulp-concat": "^2.6.0",
"gulp-ng-annotate": "^1.1.0",
"gulp-sourcemaps": "^1.6.0",
"gulp-uglify": "^1.5.1",
"gulp-util": "^3.0.7",
"gulp-watch": "^4.3.5"
}
}</pre>

&nbsp;