---
id: 48
title: 'Writing multi-threaded code in C#'
date: 2012-01-24T22:01:00+00:00
author: Manthan Dave
layout: post
guid: http://www.manthanhd.com/2012/01/24/how-do-i-do-threading-in-c/
permalink: /2012/01/24/how-do-i-do-threading-in-c/
blogger_blog:
  - codeninjutsu.blogspot.com
blogger_author:
  - ManthanHD
blogger_permalink:
  - /2012/01/how-do-i-do-threading-in-c.html
blogger_internal:
  - /feeds/2026358850785924011/posts/default/8239640308746824041
categories:
  - Findings
tags:
  - .net
  - c
---
Threading in C# is no big deal. It has the easiest implementation I've ever seen. First of all, you need to have a method that you want to execute on a different thread. For this instance, I'll put method called Calculate(). Now, this method has to be a void. However, it can take any amount of parameters. So, it can be:

<code>public void Calculate()
</code>
or

<code>public void Calculate(int x, int y)
</code>
Now that we know what to execute, we need to create a thread with this method. This is done by:

<code>Thread t = new Thread(Calculate());
</code>
I can now run the thread by typing:

<code>t.Start();
</code>
See Also: <a href="http://codeninjutsu.blogspot.com/2012/01/how-do-i-do-threaded-programming-in.html">How do I do threaded programming in Java?</a>