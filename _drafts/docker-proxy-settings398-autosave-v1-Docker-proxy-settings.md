---
id: 415
title: Docker proxy settings
date: 2016-08-27T13:20:14+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2016/08/27/398-autosave-v1/
permalink: /?p=415
---
Here's a quick way to configure docker to work with proxy. Keep in mind that this only works if you are using boot2docker or a docker-machine. This generally applies to Windows or Mac operating systems

Create a new docker-machine with proxy:
<pre class="lang:sh decode:true">docker-machine create -d virtualbox \
    --engine-env HTTP_PROXY=http://user:password@example.com:port \
    --engine-env HTTPS_PROXY=https://user:password@example.com:port \
    --engine-env NO_PROXY=example2.com \
    proxybox</pre>
Edit user, password, example.com and port variables with appropriate values. Also, make sure you have correct domains excluded within <span class="lang:default decode:true crayon-inline ">NO_PROXY</span> variable.

To change proxy credentials later on, edit <span class="lang:default decode:true crayon-inline">/var/lib/boot2docker/profile</span> and set <span class="lang:default decode:true crayon-inline ">HTTP_PROXY</span> and <span class="lang:default decode:true crayon-inline">HTTPS_PROXY</span> environment variables.