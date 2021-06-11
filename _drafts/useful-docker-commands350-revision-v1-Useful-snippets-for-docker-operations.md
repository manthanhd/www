---
id: 353
title: Useful snippets for docker operations
date: 2016-03-16T21:14:03+00:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2016/03/16/350-revision-v1/
permalink: /?p=353
---
<h1>Managing containers</h1>
Remove all old containers:
<pre class="lang:sh decode:true">docker ps -a | grep 'weeks ago' | awk '{print $1}' | xargs --no-run-if-empty docker rm</pre>
Remove all stopped containers:
<pre class="lang:sh decode:true">docker ps -a | grep 'Exited' | awk '{print $1}' | xargs --no-run-if-empty docker rm</pre>
<h1>Managing images</h1>
Remove all untagged images:
<pre class="lang:sh decode:true ">docker rmi $(docker images -a | grep "^&lt;none&gt;" | awk '{print $3}')</pre>
Remove all untagged images (force):
<pre class="lang:sh decode:true ">docker rmi -f $(docker images -a | grep "^&lt;none&gt;" | awk '{print $3}')</pre>
&nbsp;