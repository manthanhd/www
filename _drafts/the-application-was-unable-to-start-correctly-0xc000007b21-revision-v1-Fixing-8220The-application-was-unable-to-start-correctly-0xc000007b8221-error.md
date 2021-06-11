---
id: 184
title: 'Fixing &#8220;The application was unable to start correctly (0xc000007b)&#8221; error'
date: 2015-08-25T23:19:29+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2015/08/25/21-revision-v1/
permalink: /?p=184
---
So I found this error when I started Lotus Notes this morning. After looking through some forums, I found that I needed to re-install Microsoft Visual C++ 2010 (Redistributable) from <a href="http://www.microsoft.com/en-us/download/details.aspx?id=5555" target="_blank">here</a>.

This error is usually caused by C:\Windows\System32\MSVCR100.dll being corrupted. Reinstalling VC++  fixes it.