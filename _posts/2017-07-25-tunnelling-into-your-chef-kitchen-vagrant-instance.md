---
id: 634
title: Tunnelling into your Chef Kitchen Vagrant instance
date: 2017-07-25T10:04:46+01:00
author: Manthan Dave
layout: post
guid: https://www.manthanhd.com/?p=634
permalink: /2017/07/25/tunnelling-into-your-chef-kitchen-vagrant-instance/
categories:
  - Findings
tags:
  - chef
  - kitchen
  - ssh
  - tunnel
  - vagrant
---
So you've just done this:
<pre class="lang:sh decode:true ">kitchen create default-centos-72</pre>
And now something has gone wrong on the box and you need to tunnel through to check something on your instance (say mysql database). Whatever the port may be, you need to change your current directory to where the vagrant file is. This can be achieved by running the following command relative to where your kitchen file is:
<pre class="lang:sh decode:true ">cd .kitchen/kitchen-vagrant/kitchen-default-centos72</pre>
List the directory and you will be able to see a Vagrantfile in it. Once you are there, run:
<pre class="lang:sh decode:true">vagrant ssh -- -L 3306:localhost:3306</pre>
Here I'm using mysql port 3306, you can replace it with whatever port you want to tunnel through.

If you want to tunnel multiple ports, run:
<pre class="lang:sh decode:true ">vagrant ssh -- -L 3306:localhost:3306 -L 27017:localhost:27017</pre>
Repeating -L &lt;BIND_ADDRESS&gt; multiple times.