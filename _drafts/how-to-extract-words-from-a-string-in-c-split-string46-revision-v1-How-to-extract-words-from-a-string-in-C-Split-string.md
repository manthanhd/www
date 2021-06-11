---
id: 158
title: 'How to extract words from a string in C# (Split string)'
date: 2015-08-25T22:51:53+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2015/08/25/46-revision-v1/
permalink: /?p=158
---
This is incredibly easy in C#. First of all, you need to have a string from which you want to extract words. A word is a string separated by spaces. To do that, just say:

<code>string s = "Welcome to my blog!";</code>

Now, when I use the <code>Split</code> command, it will return an array of individual strings. So, the implementation will be:

<code>string[] words = s.Split(" ",StringSplitOptions.RemoveEmptyEntries);</code>

As a result, <code>words</code> array will contain "Welcome", "to", "my", "blog!".