---
id: 544
title: Setting up corporate proxy on Docker for mac
date: 2017-01-19T09:42:16+00:00
author: Manthan Dave
layout: post
guid: https://www.manthanhd.com/?p=544
permalink: /2017/01/19/setting-up-corporate-proxy-on-docker-for-mac/
image: /wp-content/uploads/2017/01/docker-turtles-banner-network-copy-800x267.jpg
categories:
  - Findings
tags:
  - docker
---
Working with Docker on corporate proxy is a painful experience. Mainly because there aren't many guides available to do it. Finally after banging my head on the desk for a long time, my friend and colleague at <a href="https://nextmetaphor.io">https://nextmetaphor.io</a> showed me how to do it.

First of all, fire up your terminal and open up docker tty in screen.
<pre class="lang:sh decode:true">screen ~/Library/Containers/com.docker.docker/Data/com.docker.driver.amd64-linux/tty</pre>
If you see a blank screen, press enter. You should see a prompt.

Make sure you are in the docker VM by typing the hostname command. You should see the response as moby. If your response is other than that, try that screen command again.
<pre class="lang:sh decode:true">/ # hostname
moby</pre>
Now we want to view docker's routing table. This is because we'll need to find out the IP address of the host machine that is running the proxy. This is specific to my setup where I have a charles proxy server running on my machine which proxies to the remote corporate proxy.
<pre class="lang:sh decode:true">/ # netstat -rn
Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
0.0.0.0         192.168.65.1    0.0.0.0         UG        0 0          0 eth0
172.17.0.0      0.0.0.0         255.255.0.0     U         0 0          0 docker0
192.168.65.0    0.0.0.0         255.255.255.240 U         0 0          0 eth0</pre>
Get the gateway entry for 0.0.0.0. In my case this is 192.168.65.1. That's the IP for the host machine running docker. For the proxy running on your local machine, just map it to the port. My charles server is running on port 8099 so my proxy will be:

http://192.168.65.1:8099

Close screen by pressing <strong>Ctrl + a \ </strong>key. Once you've exited, open up docker for mac preferences.

Go to the "advanced" tab and fill out your proxy settings.

Hit apply and restart when done!

Your Docker for Mac should now work harmoniously with your proxy!