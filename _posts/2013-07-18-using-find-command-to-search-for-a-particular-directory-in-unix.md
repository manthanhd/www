---
id: 17
title: Using find command to search for a particular directory in unix
date: 2013-07-18T07:53:00+01:00
author: Manthan Dave
layout: post
permalink: /2013/07/18/using-find-command-to-search-for-a-particular-directory-in-unix/
blogger_blog:
  - codeninjutsu.blogspot.com
blogger_author:
  - ManthanHD
blogger_permalink:
  - /2013/07/using-find-command-to-search-for.html
blogger_internal:
  - /feeds/2026358850785924011/posts/default/4436526302873941437
categories:
  - Findings
tags:
  - linux
  - shell
---
I was looking for a particular directory today on my computer and I didn't know how to do it. After spending some time searching for it, I found the following command:

<code>find [parent directory] -type d -name [directory name]</code>

So in my case:

<code>find /home/vader -type d -name wrapper_scripts</code>

Optionally you can also use:

<code>find /home/vader -type d | grep wrapper</code>