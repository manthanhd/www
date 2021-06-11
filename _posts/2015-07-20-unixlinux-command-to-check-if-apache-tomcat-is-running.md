---
id: 7
title: Unix/Linux command to check if Apache Tomcat is running
date: 2015-07-20T12:39:00+01:00
author: Manthan Dave
layout: post
permalink: /2015/07/20/unixlinux-command-to-check-if-apache-tomcat-is-running/
blogger_blog:
  - codeninjutsu.blogspot.com
blogger_author:
  - ManthanHD
blogger_permalink:
  - /2015/07/unixlinux-command-to-check-if-apache.html
blogger_internal:
  - /feeds/2026358850785924011/posts/default/1633161359645911252
categories:
  - Findings
tags:
  - linux
  - shell
  - tomcat
  - unix
---
So I've come across this problem quite a few times. Normal way to do this is:

<code>ps -ef | grep tomcat</code>

This works most of the times. If tomcat is running, it gives between 1 and 2 lines back but if not, it gives anywhere between 0 and 1 lines back. A much cleaner use of the above command would be with <code>wc -l</code>:

<code>ps -ef | grep tomcat | wc -l</code>

However, this doesn't solve the actual problem as along with the tomcat process, it also gives you the process of command <code>"grep tomcat"</code>.
<!--more-->
Here's the command to solve this problem. You can use either of the two below commands:

<code>ps -ef | grep tomcat | grep -v "grep tomcat" | wc -l</code>

<code>ps -ef | grep tomca[t] | wc -l</code>

The first command explicly says that once you get a list of all processes containing the word tomcat, ignore lines containing words <code>"grep tomcat"</code>. And then the usual, pipe it to word count and output the number of lines.

The second one, however, tricks the grep into using a regular expression and ignoring itself. This is because the actual output containing <code>"grep tomca[t]"</code> will have the square brackets which obviously won't match the actual regular expression.