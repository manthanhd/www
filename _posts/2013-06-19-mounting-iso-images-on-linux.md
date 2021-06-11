---
id: 20
title: Mounting ISO images on Linux via command line
date: 2013-06-19T14:00:00+01:00
author: Manthan Dave
layout: post
permalink: /2013/06/19/mounting-iso-images-on-linux/
blogger_blog:
  - codeninjutsu.blogspot.com
blogger_author:
  - ManthanHD
blogger_permalink:
  - /2013/06/mounting-iso-images-on-linux.html
blogger_internal:
  - /feeds/2026358850785924011/posts/default/6448385974145287976
categories:
  - Findings
tags:
  - linux
  - shell
---
So I learnt this today and decided to share it with you guys. If you have an iso image and want to mount it on Linux, simply execute the following command in terminal:

<code>sudo mount -o loop mydisk.iso myfoldername</code>

So in my case, I had iso file called 2012photos.iso on my desktop and I wanted to mount it on folder called 2012pics which was on Desktop as well. I executed:

<code>sudo mount -o loop /home/manthan/Desktop/2012photos.iso /home/manthan/Desktop/2012pics</code>