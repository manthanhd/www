---
id: 175
title: Checking memory (RAM) size on Solaris
date: 2015-08-25T23:12:52+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2015/08/25/25-revision-v1/
permalink: /?p=175
---
If you want to check how much RAM your Solaris machine has, here's how:
<code>/usr/sbin/prtdiag -v | grep -i memory</code>

You can also try:
<code>/usr/sbin/prtconf | grep -i memory</code>