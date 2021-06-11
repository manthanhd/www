---
id: 678
title: Finding classes in jar/war files
date: 2018-03-10T13:18:04+00:00
author: Manthan Dave
layout: post
guid: https://www.manthanhd.com/?p=678
permalink: /2018/03/10/finding-classes-in-jar-war-files/
categories:
  - Findings
tags:
  - automation
  - bash
  - jar
  - java
  - linux
  - shell
  - unix
---
Recently I was out looking for classes that were present in my Web Application Archive (WAR) file. Why? Well, I was out combating <a href="https://dzone.com/articles/what-is-jar-hell">Jar Hell</a>. As I am allergic to doing things manually, here's a small command I constructed to help me find classes within jar files:
<pre class="lang:sh decode:true">find . -type f -name "*.jar" -exec sh -c "jar -tf {} | grep "SomeClass.class" &amp;&amp; echo ^ found in {} file" \;</pre>
The above command prints out class file as well as the name of the jar file that class is present in. If you're looking just for the path where the classes are present, you can just run:
<pre class="lang:sh decode:true ">find . -type f -name "*.jar" | xargs -n 1 jar -tf | grep "SomeClass.class" | less</pre>
&nbsp;