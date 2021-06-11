---
id: 424
title: Useful Docker commands
date: 2016-08-27T13:23:56+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2016/08/27/350-revision-v1/
permalink: /?p=424
---
<h1>Managing containers</h1>
Remove all old containers:
<pre class="lang:sh decode:true">docker ps -a | grep 'weeks ago' | awk '{print $1}' | xargs --no-run-if-empty docker rm</pre>
Remove all stopped containers:

<!--more-->
<pre class="lang:sh decode:true">docker ps -a | grep 'Exited' | awk '{print $1}' | xargs --no-run-if-empty docker rm</pre>
<h1>Managing images</h1>
Remove all untagged images:
<pre class="lang:sh decode:true ">docker rmi $(docker images -a | grep "^&lt;none&gt;" | awk '{print $3}')</pre>
Remove all untagged images (force):
<pre class="lang:sh decode:true ">docker rmi -f $(docker images -a | grep "^&lt;none&gt;" | awk '{print $3}')</pre>
&nbsp;