---
id: 185
title: Managing password expiry for a user account (Unix/Linux)
date: 2015-08-25T23:20:00+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2015/08/25/24-revision-v1/
permalink: /?p=185
---
I find this very useful on several occasions so here we go. Login as root and execute the following:
<code>passwd -x -1 </code>

So if you want to remove password expiry from bob's account, type:
<code>passwd -x -1 bob</code>

Notice that -1 parameter after -x represents number of days before the password expires. Since we do not want the password to expire, we have set it to -1. However, you can set it to any value you want. So, if you want your password to expire after 90 days (i.e. 3 months) for bob's account, type:
<code>passwd -x 90 bob</code>