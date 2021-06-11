---
id: 413
title: Some docker helper commands
date: 2016-08-27T13:19:03+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2016/08/27/404-revision-v1/
permalink: /?p=413
---
So a lot of times when you're working with some cool docker containers, you might need to remove all those dead containers that are just lying around. This could happen if you're running docker containers in background or forgot to use <span class="lang:default decode:true crayon-inline">--rm</span> flag.

Not to worry, just run this command to remove ALL running docker containers in one go:<!--more-->
<pre class="lang:sh decode:true"> docker ps -a | cut -d ' ' -f1 | grep -v CONTAINER | xargs -n 1 -I {} docker rm {}</pre>
That will list all docker containers, cut the first column, remove the one with <span class="lang:sh decode:true crayon-inline">CONTAINER</span> in it and pipe all the remaining ones one by one using xargs into docker rm command.

Awesome right? Well, what if you only wanted to remove only the ones that have been STOPPED? Here you go:
<pre class="lang:sh decode:true ">docker ps -a | grep Exited | cut -d ' ' -f1 | grep -v CONTAINER | xargs -n 1 -I {} docker rm {}</pre>
As you can see, this command only removes the containers that have 'Exited'.

To stop all running containers:
<pre class="lang:sh decode:true ">docker ps -a | grep Up | cut -d ' ' -f1 | grep -v CONTAINER | xargs -n 1 -I {} docker stop {}</pre>
Some helper functions:
<pre class="lang:sh decode:true">function dockstopall() {
    docker ps -a | grep Up | cut -d ' ' -f1 | grep -v CONTAINER | xargs -n 1 -I {} docker stop {}
}

function dockrmall() {
    docker ps -a | grep Exited | cut -d ' ' -f1 | grep -v CONTAINER | xargs -n 1 -I {} docker rm {}
}</pre>
Add these to your <span class="lang:default decode:true crayon-inline ">.profile</span> or <span class="lang:default decode:true crayon-inline ">.bash_profile</span> or whatever shell profile you use. Once added, to apply in existing shell, run:
<pre class="lang:sh decode:true ">source ~/.profile</pre>
&nbsp;