---
id: 155
title: Preventing class redefinition errors in C++
date: 2015-08-25T22:49:00+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2015/08/25/31-revision-v1/
permalink: /?p=155
---
I have just started learning C++ and have been getting a lot of these errors. I finally found a solution to this after spending some time on the internet.

Say for instance you have a class called "Animal" and g++ complains that this class is being redefined. Just add the following two lines at the very top of the Animal.h file:

<code>#ifndef ANIMAL_H
#define ANIMAL_H</code>

and add the following at the very bottom of the file:
<code>
#endif</code>

So, if your class is called SomeClass, then it will be SOMECLASS_H instead of ANIMAL_H. You should have these in every header file. It prevents that header file from being redefined more than once.