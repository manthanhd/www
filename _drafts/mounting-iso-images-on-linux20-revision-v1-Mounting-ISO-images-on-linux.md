---
id: 83
title: Mounting ISO images on linux
date: 2013-06-19T14:00:00+01:00
author: Manthan Dave
layout: revision
guid: http://www.manthanhd.com/2013/06/19/20-revision-v1/
permalink: /?p=83
---
So I learnt this today and decided to share it with you guys. If you have an iso image and want to mount it on Linux, simply execute the following command in terminal:<br /><br /><code>sudo mount -o loop mydisk.iso myfoldername</code><br /><br />So in my case, I had iso file called 2012photos.iso on my desktop and I wanted to mount it on folder called 2012pics which was on Desktop as well. I executed:<br /><br /><code>sudo mount -o loop /home/manthan/Desktop/2012photos.iso /home/manthan/Desktop/2012pics</code>