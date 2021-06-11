---
id: 180
title: Using find command to search for a particular directory in unix
date: 2015-08-25T23:17:43+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2015/08/25/17-revision-v1/
permalink: /?p=180
---
I was looking for a particular directory today on my computer and I didn't know how to do it. After spending some time searching for it, I found the following command:

<code>find [parent directory] -type d -name [directory name]</code>

So in my case:

<code>find /home/vader -type d -name wrapper_scripts</code>

Optionally you can also use:

<code>find /home/vader -type d | grep wrapper</code>