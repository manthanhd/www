---
id: 31
title: Preventing class redefinition errors in C++
date: 2013-04-14T01:48:00+01:00
author: Manthan Dave
layout: post
guid: http://www.manthanhd.com/2013/04/14/how-to-prevent-class-redefinition-errors-in-c/
permalink: /2013/04/14/how-to-prevent-class-redefinition-errors-in-c/
blogger_blog:
  - codeninjutsu.blogspot.com
blogger_author:
  - ManthanHD
blogger_permalink:
  - /2013/04/how-to-prevent-class-redefinition.html
blogger_internal:
  - /feeds/2026358850785924011/posts/default/5654206864253185872
categories:
  - Findings
tags:
  - c
---
I have just started learning C++ and have been getting a lot of these errors. I finally found a solution to this after spending some time on the internet.

Say for instance you have a class called "Animal" and g++ complains that this class is being redefined. Just add the following two lines at the very top of the Animal.h file:

<code>#ifndef ANIMAL_H
#define ANIMAL_H</code>

and add the following at the very bottom of the file:
<code>
#endif</code>

So, if your class is called SomeClass, then it will be SOMECLASS_H instead of ANIMAL_H. You should have these in every header file. It prevents that header file from being redefined more than once.