---
id: 615
title: Appending to crontab using a single shell command
date: 2017-04-24T10:00:13+01:00
author: Manthan Dave
layout: post
guid: https://www.manthanhd.com/?p=615
permalink: /2017/04/24/appending-to-crontab-using-a-single-shell-command/
categories:
  - Educational
tags:
  - cron
  - linux
  - shell
  - unix
---
Usually to edit crontab for a user, you login as that user and then run:
<pre class="lang:sh decode:true ">crontab -e</pre>
This usually opens up a text editor which then lets you edit the crontab. Once you are done, you save and quit, and this magically updates your crontab.

Today I was writing a script that needed to update crontab without any user interaction. After doing some digging, I found this neat way of updating my crontab;
<pre class="lang:sh decode:true">(crontab -l ; echo "* 1 * * 1 /usr/bin/letsencrypt renew &amp;&amp; service nginx restart")| crontab -</pre>
The above example is straight out of my shell script which renews my letsencrypt certificate and then restarts the nginx server.