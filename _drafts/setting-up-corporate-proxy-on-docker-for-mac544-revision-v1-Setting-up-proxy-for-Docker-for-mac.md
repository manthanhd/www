---
id: 545
title: Setting up proxy for Docker for mac
date: 2017-01-18T15:29:48+00:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2017/01/18/544-revision-v1/
permalink: /?p=545
---
Run
screen ~/Library/Containers/com.docker.docker/Data/com.docker.driver.amd64-linux/tty

When screen appears, press enter. You should see a prompt.

Make sure you are in the docker VM by typing
/ # hostname
moby

Type
netstat -rn

to view routing table

/ # netstat -rn
Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
0.0.0.0         192.168.65.1    0.0.0.0         UG        0 0          0 eth0
172.17.0.0      0.0.0.0         255.255.0.0     U         0 0          0 docker0
192.168.65.0    0.0.0.0         255.255.255.240 U         0 0          0 eth0

Get the gatway entry for 0.0.0.0. In my case this is 192.168.65.1. That's the IP for the host. For the proxy running on your local machine, just map it to the port.

So in the docker settings, your proxy will be:
http://192.168.65.1:<PORT>