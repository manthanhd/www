---
id: 49
title: Multi-threaded programming in Java
date: 2012-01-24T20:55:00+00:00
author: Manthan Dave
layout: post
guid: http://www.manthanhd.com/2012/01/24/how-do-i-do-threaded-programming-in-java/
permalink: /2012/01/24/how-do-i-do-threaded-programming-in-java/
blogger_blog:
  - codeninjutsu.blogspot.com
blogger_author:
  - ManthanHD
blogger_permalink:
  - /2012/01/how-do-i-do-threaded-programming-in.html
blogger_internal:
  - /feeds/2026358850785924011/posts/default/5892666432016105798
categories:
  - Findings
tags:
  - java
---
My favorite concept in any programming language is Threading. It is awesome. This time, I'll get started straight away!

First of all, before making a thread in java, you need a runnable. Runnable is basically like a piece of code that you want to execute as a separate thread. There are two ways to create a runnable. You can do it by making an anonymous class such as:

<code>Runnable r = new Runnable(){</code>
<code>
public void run(){
</code>
<code> //Your code goes here...<span style="white-space: pre;">
</span></code>
<code> }
}
</code>However, this is not a good practice. Hence, it is advised to create a separate class which implements the Runnable interface like:

<code></code>
<code>public class MyNewRunnable implements Runnable{</code>
<code>
</code>
<code> MyNewRunnable(){</code>
<code>
</code>
<code> }</code>
<code>
</code>
<code> public void run(){</code>
<code> //Your code here...</code>
<code> }</code>
<code>}</code>
<div><code> </code></div>
<code></code>In this manner, you can pass any parameters for processing the information using the class constructor. For instance, if you are making a runnable for downloading files, you can pass url of the file through the class constructor. Now, once you have created your runnable, you need a thread in which you have to put the runnable. Then you run the thread, the run method of runnable gets executed. For runnable r, you can make a thread as follows:

<code>Thread t = new Thread(r);
</code>
Then, you can start the thread by typing:

<code>t.start();
</code>
I'd say that threading would help make your program more responsive. This is solely because of the fact that all the heavy processor hungry tasks are carried out in a separate thread and the UI thread is kept clean.

See Also: <a href="http://codeninjutsu.blogspot.com/2012/01/how-do-i-do-threading-in-c.html">How do I do threading in C#?</a>