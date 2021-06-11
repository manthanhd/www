---
id: 628
title: 'Fixing ubuntu desktop&#8217;s wifi disconnect issue on wake'
date: 2017-05-11T06:33:47+01:00
author: Manthan Dave
layout: post
guid: https://www.manthanhd.com/?p=628
permalink: /2017/05/11/fixing-ubuntu-desktops-wifi-disconnect-issue-on-wake/
categories:
  - Findings
tags:
  - linux
  - troubleshooting
  - ubuntu
  - unix
  - wifi
---
For about more than half the time, when I wake my ubuntu laptop from sleep, I will lose wifi. When I say lose, I mean I lose the <code>wlan0</code> interface completely.

Now this is a problem because I quite like having wifi and normally there seems to be no way to turn it back on from that little wifi menu from the top right corner.

After doing some googling, I found a crude yet simple solution. Restart the <code>network-manager</code> service by running the following:
<pre class="lang:sh decode:true ">sudo service network-manager restart</pre>
Right before writing this article, I had to run that command and now it all works buttery smooth. Maybe there is a more permanent fix, I will keep an eye out for it and when I can find some time, will dig in to some logs somewhere to actually find the issue but for now, this is satisfactory (meh) to my current needs.