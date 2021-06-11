---
id: 38
title: Using Timer in Java
date: 2012-04-17T10:03:00+01:00
author: Manthan Dave
layout: post
guid: http://www.manthanhd.com/2012/04/17/how-to-use-timers-in-java/
permalink: /2012/04/17/how-to-use-timers-in-java/
blogger_blog:
  - codeninjutsu.blogspot.com
blogger_author:
  - ManthanHD
blogger_permalink:
  - /2012/04/how-to-use-timers-in-java.html
blogger_internal:
  - /feeds/2026358850785924011/posts/default/8133139268615345045
categories:
  - Findings
tags:
  - java
---
Well, this post will be similar to the one about Threading. However, it is an important concept and thus needs to be covered. First of all, to time a certain task, you will have to make a Timer object. However, a timer object needs to have a TimerTask object. Now, TimerTask is an abstract class and has a run method which gets called on set frequency. So, for instance, if the frequency of the task has been set to 2000 ms, then the run method gets called every 2000 ms. To use Timers, you will have to create your own class which extends the TimerTask class. This would be something like:

<code> public class SomeTask extends TimerTask{
@Override
public void run() {
//Something to do here...
}
}</code>

If you are going to access UI elements of a particular JFrame from your SomeTask class, then you will have to pass a reference of that JFrame in its class constructor. This is because all your UI elements are private and thus cannot be accessed by an external class. You will also have to make methods which would access your private UI elements in the NewJFrame. For a JFrame called NewJFrame, the code would look like:

<code>public class SomeTask extends TimerTask{

NewJFrame parent;
public SomeTask(NewJFrame frame){
parent = frame;
}

@Override
public void run() {
//Do work here
}
}</code>

If you want to type Hello every five seconds to a JTextArea in NewJFrame, then you'll first have to create a method that would do that in your NewJFrame. This method can be as simple as:

<code>public void type(){
jTextArea1.append("Hellon");
}</code>

Now, access this method from the run() method of your timer task simply by:

<code>parent.type();</code>

To run this task, you will have to make a timer by:

<code>Timer t = new Timer();</code>

Then, create a new object of SomeTask class that you just created.

<code>SomeTask st = new SomeTask(this);</code>

Notice 'this' in the constructor. It is because we asked for it in the class constructor as the task needs to access the UI elements. Now schedule that task by:

<code>t.schedule(st,0,2000);</code>

The above line takes three parameters. First is the TimerTask object, which in this case is the SomeTask object called st. Second is the intial delay (in ms) which is concerned with how long will it take to start the task for the first time and the third parameter is the frequency of the task which in this case is 2000 ms.

Thats it! Now you have scheduled your task and it will run almost instantly (0 ms delay). It will keep running every 2 seconds (2000 ms) until someone calls:

<code>st.cancel();</code>

Which will stop the task from running.

Watch the video tutorial here:

<center><iframe width="480" height="360" allowfullscreen="allowfullscreen" frameborder="0" src="http://www.youtube.com/embed/-loYJQsE3TY"></iframe></center>