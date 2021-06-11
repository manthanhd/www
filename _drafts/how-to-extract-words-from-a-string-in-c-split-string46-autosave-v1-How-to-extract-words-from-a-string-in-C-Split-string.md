---
id: 157
title: 'How to extract words from a string in C# (Split string)'
date: 2015-08-25T22:50:16+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2015/08/25/46-autosave-v1/
permalink: /?p=157
---
This is incredibly easy in C#. First of all, you need to have a string from which you want to extract words. A word is a string separated by spaces. To do that, just say:<br /><br />string s = "Welcome to my blog!";<br /><br />Now, when I use the split command, it will return an array of individual strings. So, the implementation will be:<br /><br />string[] words = s.Split(" ",StringSplitOptions.RemoveEmptyEntries);<br /><br />As a result, words will have "Welcome", "to", "my", "blog!" in it.