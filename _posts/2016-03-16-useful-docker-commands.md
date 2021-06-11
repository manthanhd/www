---
id: 350
title: Useful Docker commands
date: 2016-03-16T21:23:40+00:00
author: Manthan Dave
layout: post
guid: https://www.manthanhd.com/?p=350
permalink: /2016/03/16/useful-docker-commands/
categories:
  - Educational
  - Findings
tags:
  - docker
  - linux
  - unix
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