---
id: 24
title: Managing password expiry for a user account (Unix/Linux)
date: 2013-05-11T20:41:00+01:00
author: Manthan Dave
layout: post
permalink: /2013/05/11/managing-password-expiry-for-a-user-account-unixlinux/
blogger_blog:
  - codeninjutsu.blogspot.com
blogger_author:
  - ManthanHD
blogger_permalink:
  - /2013/05/managing-password-expiry-for-user.html
blogger_internal:
  - /feeds/2026358850785924011/posts/default/4101884874966504931
categories:
  - Findings
tags:
  - linux
  - shell
  - unix
---
I find this very useful on several occasions so here we go. Login as root and execute the following:
<code>passwd -x -1 </code>

So if you want to remove password expiry from bob's account, type:
<code>passwd -x -1 bob</code>

Notice that -1 parameter after -x represents number of days before the password expires. Since we do not want the password to expire, we have set it to -1. However, you can set it to any value you want. So, if you want your password to expire after 90 days (i.e. 3 months) for bob's account, type:
<code>passwd -x 90 bob</code>