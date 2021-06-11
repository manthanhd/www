---
id: 515
title: Gradle multi module projects
date: 2016-12-05T12:42:33+00:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2016/12/05/509-revision-v1/
permalink: /?p=515
---
Recently I've had to deal with a lot of gradle projects, specifically the multi-module ones. It's dead easy to setup, however, every time I do it, I have to refer to one of my old projects to see the layout. So, hopefully, while this document will certainly help future me, I hope it is of help to you too.

First things first, create a project directory. We will refer this directory as 'project root directory' in future. For all the awesome things that I do, we'll call this one, <strong>*drum roll please* ...</strong> awesomeproject.

<!--more-->
<pre class="lang:sh decode:true">mkdir awesomeproject
cd awesomeproject</pre>
Once in that directory, lets create the <span class="lang:sh decode:true crayon-inline ">build.gradle</span> file at the project root with the following content:
<pre class="lang:default decode:true" title="build.gradle">// build.gradle
apply plugin: 'distribution'</pre>
Alongside this, create a <span class="lang:sh decode:true crayon-inline ">settings.gradle</span> file but leave it blank for now.
<pre class="lang:sh decode:true">touch settings.gradle
</pre>
Now create your sub-module project directories. In this example, we'll create two directories, awesomeproject-scripts which will hold our deployment scripts and awesomeproject-service which will contain our actual java code.
<pre class="lang:sh decode:true">mkdir awesomeproject-scripts awesomeproject-service</pre>
Time to create gradle files for your sub-modules. We won't go into the detail of what the gradle files will actually contain, however, for semi-completeness, we'll just apply some plugins as a marker to indicate the type of sub-module projects.
<pre class="lang:default decode:true" title="awesomeproject-scripts/build.gradle">// awesomeproject-scripts/build.gradle
apply plugin: 'distribution'</pre>
<pre class="lang:default decode:true" title="awesomeproject-service/build.gradle">// awesomeproject-service/build.gradle
apply plugin: 'java'</pre>
As you can see from above, our <span class="lang:sh decode:true crayon-inline ">awesomeproject-scripts/build.gradle</span> file applies the distribution plugin to indicate that it will need to archive its contents while the <span class="lang:sh decode:true crayon-inline ">awesomeproject-service/build.gradle</span> applies the java plugin to indicate that it will be compiling its java source.

Amazing! Now we just need to tell our root project to include these two sub-modules. This is simple. You know we left that <span class="lang:sh decode:true crayon-inline ">settings.gradle</span> file in the project root blank? Well, lets change that.
<pre class="lang:default decode:true" title="settings.gradle">// settings.gradle
include 'awesomeproject-scripts', 'awesomeproject-service'</pre>
Here, we are telling the root gradle project to include the sub-modules. Gradle will then expect <span class="lang:sh decode:true crayon-inline">build.gradle</span> files within these directories and will execute jobs. It's pretty clever so if you've got a module that needs to execute one of the other sub-modules first, it will run it in the correct required order.

More <a href="https://docs.gradle.org/current/userguide/multi_project_builds.html">detailed documentation</a> on this is available on gradle's website, however, this should get you up and running quickly.