---
id: 551
title: Fixing docker service startup after using an alternative graph driver
date: 2017-04-15T12:02:18+01:00
author: Manthan Dave
layout: post
guid: https://www.manthanhd.com/?p=551
permalink: /2017/04/15/fixing-docker-service-startup-after-using-an-alternative-graph-driver/
categories:
  - Findings
tags:
  - docker
  - linux
  - troubleshooting
---
So I was playing around with devicemapper docker storage driver the other day. It was quite nice but when I stopped the modified docker daemon and tried to start the docker service in "normal" way, I received the following error:
<pre class="lang:default decode:true ">root@carbon:~# service docker start
Job for docker.service failed because the control process exited with error code. See "systemctl status docker.service" and "journalctl -xe" for details.</pre>
Clearly something had gone wrong. Upon running:
<pre class="lang:sh decode:true ">systemctl status docker.service</pre>
I found the following line most helpful in fixing the error:
<pre class="lang:default decode:true ">Jan 24 22:33:17 carbon dockerd[3753]: Error starting daemon: error initializing graphdriver: /var/lib/docker contains several valid graphdrivers: devicemapper, aufs; Please cleanup or explicitly choose storage driver (-s</pre>
So I ran the following command in order to remove my devicemapper driver:
<pre class="lang:sh decode:true ">rm -rf /var/lib/docker/devicemapper</pre>
Boom it worked!

Alternatively, I could've backed it up to tmp instead by running:
<pre class="lang:sh decode:true ">mv /var/lib/docker/devicemapper /tmp/</pre>
But I didn't need any of my images anyway so I removed it.

Hope this helps.