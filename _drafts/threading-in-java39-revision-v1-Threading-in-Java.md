---
id: 191
title: Threading in Java
date: 2015-08-25T23:24:06+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2015/08/25/39-revision-v1/
permalink: /?p=191
---
Every thread in Java needs a Runnable. To make a runnable, you must first create a class which implements the Runnable interface. Runnable interface has a method called <code>run()</code> which gets called by the Thread. Your class will need to have this method. Put your code in this <code>run()</code> method. Your Runnable class should look similar to:
<pre class="lang:java">public class Work implements Runnable{

@Override
public void run() {
//do some work..
}
}
</pre>
Now, you can create a thread in multiple ways but the most appropriate way is to pass the runnable object in its constructor.
<pre class="lang:java">Thread thread1 = new Thread(new Work());
Work work = new Work();
Thread thread2 = new Thread(work);</pre>
<pre class="lang:java">thread1.start();
thread2.start();
</pre>
Make sure that you say <code>thread1.start()</code> and not <code>thread1.run()</code>. <code>thread1.run()</code> simply calls the run method within the runnable while <code>thread1.start()</code> runs the run method in separate thread.

That's it! Now you know the basics of threading in Java!

Check out the video tutorial here:
<iframe width="640" height="360" allowfullscreen="allowfullscreen" frameborder="0" src="http://www.youtube.com/embed/videoseries?list=PL4DD8CFBE563F29AC&amp;hl=en_US"></iframe>