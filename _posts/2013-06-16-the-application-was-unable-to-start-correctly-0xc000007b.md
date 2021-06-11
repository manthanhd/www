---
id: 21
title: 'Fixing &#8220;The application was unable to start correctly (0xc000007b)&#8221; error'
date: 2013-06-16T11:11:00+01:00
author: Manthan Dave
layout: post
permalink: /2013/06/16/the-application-was-unable-to-start-correctly-0xc000007b/
blogger_blog:
  - codeninjutsu.blogspot.com
blogger_author:
  - ManthanHD
blogger_permalink:
  - /2013/06/the-application-was-unable-to-start.html
blogger_internal:
  - /feeds/2026358850785924011/posts/default/8493850457257136996
categories:
  - Findings
tags:
  - troubleshooting
  - windows
---
So I found this error when I started Lotus Notes this morning. After looking through some forums, I found that I needed to re-install Microsoft Visual C++ 2010 (Redistributable) from <a href="http://www.microsoft.com/en-us/download/details.aspx?id=5555" target="_blank">here</a>.

This error is usually caused by C:\Windows\System32\MSVCR100.dll being corrupted. Reinstalling VC++  fixes it.