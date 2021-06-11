---
id: 84
title: The application was unable to start correctly (0xc000007b)
date: 2013-06-16T11:11:00+01:00
author: Manthan Dave
layout: revision
guid: http://www.manthanhd.com/2013/06/16/21-revision-v1/
permalink: /?p=84
---
So I found this error when I started Lotus Notes this morning. After looking through some forums, I found that I needed to re-install Microsoft Visual C++ 2010 (Redistributable) from <a href="http://www.microsoft.com/en-us/download/details.aspx?id=5555" target="_blank">here</a>.<br /><br />This error is usually caused by C:WindowsSystem32MSVCR100.dll being corrupted. Reinstalling VC++ &nbsp;fixes it.