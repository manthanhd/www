---
id: 593
title: Which process is using this port?
date: 2017-04-11T07:35:50+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2017/04/11/567-revision-v1/
permalink: /?p=593
---
Sometimes, I get errors like "address is already in use" but struggle to find out what's using that port. This happens especially with loads of stuff running in background like docker, vagrant vms, local server instances, ssh tunnels etc.

Here's an easy way to figure out whats running on a certain port:
<pre class="lang:sh decode:true">netstat -vanp tcp | grep 8080</pre>
The second last column refers to the process using that port.

For example, running that on my mac:
<pre class="lang:sh decode:true">➜ ~ netstat -vanp tcp | grep 8080
tcp4 0 0 *.8080 *.* LISTEN 65536 65536 5500 0</pre>
Indicates that process ID <span class="lang:default decode:true  crayon-inline ">5500</span> is using port <span class="lang:default decode:true  crayon-inline">8080</span>. Doing a process check on that tells me:
<pre class="lang:sh decode:true">➜ ~ ps -ef | grep 5500
394661014 5500 5443 0 7:15am ?? 0:42.61 /Applications/VirtualBox.app/Contents/MacOS/VBoxHeadless --comment kitchen-awesomevm --startvm b459e3ea-566b-4184-a109-cf97338958aa --vrde config</pre>
Hope this helps!