---
id: 25
title: Checking memory (RAM) size on Solaris
date: 2013-05-07T18:10:00+01:00
author: Manthan Dave
layout: post
guid: http://www.manthanhd.com/2013/05/07/checking-memory-ram-size-on-solaris/
permalink: /2013/05/07/checking-memory-ram-size-on-solaris/
blogger_blog:
  - codeninjutsu.blogspot.com
blogger_author:
  - ManthanHD
blogger_permalink:
  - /2013/05/checking-memory-ram-size-on-solaris.html
blogger_internal:
  - /feeds/2026358850785924011/posts/default/1055570086594828768
categories:
  - Findings
tags:
  - solaris
  - unix
---
If you want to check how much RAM your Solaris machine has, here's how:
<code>/usr/sbin/prtdiag -v | grep -i memory</code>

You can also try:
<code>/usr/sbin/prtconf | grep -i memory</code>