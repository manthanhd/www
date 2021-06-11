---
id: 51
title: Using Sessions in ASP .NET
date: 2012-01-20T22:50:00+00:00
author: Manthan Dave
layout: post
guid: http://www.manthanhd.com/2012/01/20/using-sessions-in-asp-net/
permalink: /2012/01/20/using-sessions-in-asp-net/
blogger_blog:
  - codeninjutsu.blogspot.com
blogger_author:
  - ManthanHD
blogger_permalink:
  - /2012/01/using-sessions-in-asp-net.html
blogger_internal:
  - /feeds/2026358850785924011/posts/default/7969085552391363858
categories:
  - Findings
tags:
  - .net
  - asp
  - c
---
Well, believe it or not, Session is the most important thing when it comes to web design. I used to hate it but now I am a big fan of it. Trust me. You will like it once you get hang of it. So, in this post, I will be teaching you about using them. First of all, a session is like a small memory pit where you can store values. Values in session are lost once the browser closes. However, they stay the same even if you change the page or open new tab. Hence, sessions are used to pass values from one page to another.

You can store an object in session. It can be int, double, string or any other FooClass object that you have defined. This is the main benefit of it. However, when taking values back, you need to make sure that you cast them back to its same original form. Here's how you store value in a session. Let me define some variables.

<code>int i=2;
string s="Hello";
FooClass foo = new FooClass();
</code>

Now, I will store each of them in session. To store a value, you need to give a session name. This must be unique. You must remember this because you will have to use it to extract value out of session. Here's how you do it:

<code>Session["someIntegerVariable"] = i;
Session["someStringVariable"] = s;
Session["someFooClassObject"] = foo;
</code>

Now that I have all of them in the session, I will extract them. Remember, I can only extract variable 'i' from 'someIntegerVariable' session and not 'someStringVariable' session.

<code>int newI = (int)Session["someIntegerVariable"];
string newS = (string)Session["someStringVariable"];
FooClass = (FooClass)Session["someFooClassObject"];
</code>

Since session stores object only, you will have to cast the object back to its original form. In the code above, I have casted the object stored in 'someIntegerVariable' session back to its original form which is int.

That's basically how you use sessions. It is widely used in tracking pages and variety of other purposes, especially when you have to keep something consistent between pages.