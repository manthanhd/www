---
id: 46
title: 'How to extract words from a string in C# (Split string)'
date: 2012-01-31T15:48:00+00:00
author: Manthan Dave
layout: post
guid: http://www.manthanhd.com/2012/01/31/how-to-extract-words-from-a-string-in-c-split-string/
permalink: /2012/01/31/how-to-extract-words-from-a-string-in-c-split-string/
blogger_blog:
  - codeninjutsu.blogspot.com
blogger_author:
  - ManthanHD
blogger_permalink:
  - /2012/01/how-to-extract-words-from-string-in-c.html
blogger_internal:
  - /feeds/2026358850785924011/posts/default/2092040013957242859
categories:
  - Findings
tags:
  - .net
  - c
---
This is incredibly easy in C#. First of all, you need to have a string from which you want to extract words. A word is a string separated by spaces. To do that, just say:

<code>string s = "Welcome to my blog!";</code>

Now, when I use the <code>Split</code> command, it will return an array of individual strings. So, the implementation will be:

<code>string[] words = s.Split(" ",StringSplitOptions.RemoveEmptyEntries);</code>

As a result, <code>words</code> array will contain "Welcome", "to", "my", "blog!".