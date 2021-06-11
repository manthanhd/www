---
id: 19
title: Copying symbolic links (Unix/Linux)
date: 2013-06-20T21:25:00+01:00
author: Manthan Dave
layout: post
permalink: /2013/06/20/copying-symbolic-links-unixlinux/
blogger_blog:
  - codeninjutsu.blogspot.com
blogger_author:
  - ManthanHD
blogger_permalink:
  - /2013/06/copying-symbolic-links-unixlinux.html
blogger_internal:
  - /feeds/2026358850785924011/posts/default/2365865146120906025
categories:
  - Findings
tags:
  - linux
  - shell
---
At some point, you might come across a situation where you have some files and a bunch of symlinks (symbolic links) in a folder and you wish to copy the data that is pointed at by the symlinks and not just blank links. If you are using Redhat Enterprise Linux, this should automatically happen when you call the cp command. However, on UNIX platforms or Linux distributions, this is not the case by default. In those cases, you will have to issue the following command:
<!--more-->

<code>cp -L {file(s)} {destination folder}</code>
<code>
</code>The -L option in cp command forces it to follow (dereference) symlinks and copy the data that is pointed at by the symlink.

In case you do not want to follow symlinks, you can issue the following command instead:
<code>
</code><code>cp -P {file(s)} {destination folder}</code><span style="font-family: monospace;"> </span>