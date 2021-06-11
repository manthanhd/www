---
id: 54
title: Code efficiency and The Big O Notation
date: 2012-01-19T17:56:00+00:00
author: Manthan Dave
layout: post
guid: http://www.manthanhd.com/2012/01/19/introduction-to-code-efficiency-and-the-big-o-notation/
permalink: /2012/01/19/introduction-to-code-efficiency-and-the-big-o-notation/
blogger_blog:
  - codeninjutsu.blogspot.com
blogger_author:
  - ManthanHD
blogger_permalink:
  - /2012/01/introduction-to-code-efficiency-and-big.html
blogger_internal:
  - /feeds/2026358850785924011/posts/default/8320883813510830680
categories:
  - Findings
  - Thoughts
tags:
  - code efficiency
---
The big O notation explains how efficient the code is. It is like measurement scale for code efficiency. If you go in details, it can become complex. However, in this post, I'll explain how to get a rough idea of your code's efficiency.

To roughly calculate it, you need to know size of your problem which is 'n' in this case. Look through your code, and analyze the loops that you have. Basically, loops are the determining factor. If your code has 100 loops (not nested) then the efficiency is O(n). No matter what the number is, if your code has no nested loops, then the efficiency is still O(n). However, if you have one single nested loop, then efficiency is n2. Nested loops decrease efficiency of code. This is simply depicted by the fact that n2 is greater than n.

To write efficient code, it should be your goal to reduce number of nested loops. To do this, the best way is to reuse values that have been previously generated. My way to do this is by using connected methods. I will be discussing more about this in next posts. You can also exit loops when you have met your aim. In this case, while loop is preferred but if you have more than one condition, you should exit the loop when they are met just to save iterations.

If you wish to know more about the big O notation, read the <a href="http://rob-bell.net/2009/06/a-beginners-guide-to-big-o-notation/">article written by Rob Bell</a>