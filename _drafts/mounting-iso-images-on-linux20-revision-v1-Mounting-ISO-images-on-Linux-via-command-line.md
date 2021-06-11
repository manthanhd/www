---
id: 182
title: Mounting ISO images on Linux via command line
date: 2015-08-25T23:18:47+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2015/08/25/20-revision-v1/
permalink: /?p=182
---
So I learnt this today and decided to share it with you guys. If you have an iso image and want to mount it on Linux, simply execute the following command in terminal:

<code>sudo mount -o loop mydisk.iso myfoldername</code>

So in my case, I had iso file called 2012photos.iso on my desktop and I wanted to mount it on folder called 2012pics which was on Desktop as well. I executed:

<code>sudo mount -o loop /home/manthan/Desktop/2012photos.iso /home/manthan/Desktop/2012pics</code>