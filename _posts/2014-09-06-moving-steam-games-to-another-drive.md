---
id: 9
title: Moving steam games to another drive
date: 2014-09-06T22:45:00+01:00
author: Manthan Dave
layout: post
permalink: /2014/09/06/moving-steam-games-to-another-drive/
blogger_blog:
  - codeninjutsu.blogspot.com
blogger_author:
  - ManthanHD
blogger_permalink:
  - /2014/09/moving-steam-games-to-another-drive.html
blogger_internal:
  - /feeds/2026358850785924011/posts/default/1484582607831720428
geo_latitude:
  - "55.378051"
geo_longitude:
  - "-3.435973"
geo_public:
  - "1"
geo_address:
  - United Kingdom
categories:
  - Findings
tags:
  - steam
  - troubleshooting
  - windows
---
So I came across this recently and thought to share it with you guys.

First of all, exit steam completely. If you've closed it, check if it's in the system tray and if it is, exit from there as well. Now, fire up Windows Explorer and go to where steam is installed on your computer. In my case, it was in the default installation directory (C:\Program Files (x86)\Steam).
<!--more-->
Once in the folder, delete everything EXCEPT the SteamApps folder and Steam.exe. Now, go UP a directory level and move the whole Steam folder to wherever you want it to be. In my case, I moved it to E:Games folder.

When the move is complete, double-click on the steam.exe. It'll re-download steam client for you and will require you to login to your steam account again.

So that's it. You should have all your games available to you in your new drive location!

Original source: <a href="https://support.steampowered.com/kb_article.php?ref=7418-YUBN-8129">https://support.steampowered.com/kb_article.php?ref=7418-YUBN-8129</a>