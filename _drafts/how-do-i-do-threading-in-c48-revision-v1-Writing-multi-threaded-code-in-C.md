---
id: 159
title: 'Writing multi-threaded code in C#'
date: 2015-08-25T22:52:48+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2015/08/25/48-revision-v1/
permalink: /?p=159
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