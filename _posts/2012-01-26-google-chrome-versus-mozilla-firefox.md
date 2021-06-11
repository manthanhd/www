---
id: 47
title: Google Chrome vs Mozilla Firefox
date: 2012-01-26T12:43:00+00:00
author: Manthan Dave
layout: post
guid: http://www.manthanhd.com/2012/01/26/google-chrome-versus-mozilla-firefox/
permalink: /2012/01/26/google-chrome-versus-mozilla-firefox/
blogger_blog:
  - codeninjutsu.blogspot.com
blogger_author:
  - ManthanHD
blogger_permalink:
  - /2012/01/google-chrome-versus-mozilla-firefox.html
blogger_internal:
  - /feeds/2026358850785924011/posts/default/6757322836653814033
categories:
  - Thoughts
tags:
  - chrome
  - firefox
  - web browser
---
Well, they said that Mozilla firefox is the most efficient browser. I wasn't satisfied. Hence, I went to check. I am a big fan of Google Chrome. However, I recently learnt that chrome collects browsing information. That didn't sound good to me and hence I switched to Mozilla Firefox. On the download page, it said that it is the most efficient browser. I downloaded it, but initially I had some issues. It lagged a bit when I used it on my laptop. However, when I used it on my university's mac (with Windows 7 installed) it was smooth. Hence I was suspicious. I had to check. I closed all applications, refreshed the page few times and fired up Mozilla Firefox and Google Chrome.

I opened up www.sciencedaily.com in both the browsers. I fired up Task Manager only to find that Google chrome had three simultaneous processes and firefox had one. This probably means that it is doing multi-threading a lot and has a lot of external handlers. It is a good practice though when you are programming something like a web browser. However, I counted the memory allocated and firefox had like 88,000 and Google Chrome had in total something like 50,000. I was surprised.

Then, to satisfy myself that Firefox is better, I opened two tabs in both chrome and firefox. In one tab, I opened the google home page and in another I opened up www.sciencedaily.com. This time, I hoped chrome to jump up. The task manager showed that chrome had four processes, one more than last time I had seen and firefox still had one process. In terms of memory, firefox allocated something like 120,000 and chrome took something like 90,000. I was even more surprised. My quick analysis at that point was that the memory print of chrome increases rapidly with increase in number of tabs while in same conditions, that of firefox increases at relatively slower rate.

In the third test, in which I seriously hoped firefox to win, I opened up four tabs as follows:
1. Tab 1: www.google.com
2. Tab 2: www.sciencedaily.com
3. Tab 3: www.bing.com
4. Tab 4: news.google.com

This time, firefox really won. Its memory print was around 143,000 while at the same time, that of chrome was around 150,000. Yayy! Victory at last!

So, well, what does this tell to average computer user? I will still be using firefox. However, if I quickly want to check something like a definition or address, I'd use chrome. However, for tab intensive stuff like research and like that, I will be using firefox.

<b>FYI:</b>
Chrome: 12.0.742.122
Firefox: 5.0