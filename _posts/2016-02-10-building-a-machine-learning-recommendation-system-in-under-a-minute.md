---
id: 313
title: Building a machine learning recommendation system in under a minute
date: 2016-02-10T05:45:31+00:00
author: Manthan Dave
layout: post
guid: https://www.manthanhd.com/?p=313
permalink: /2016/02/10/building-a-machine-learning-recommendation-system-in-under-a-minute/
categories:
  - Findings
tags:
  - java
  - machine learning
  - mahout
---
Theoratically there are only two steps to this (well, three if you count the <span class="lang:default decode:true crayon-inline">cd</span>).
Clone GitHub repository available <a href="https://github.com/manthanhd/apache-mahout-recommendation-starter" target="_blank">here</a>.
<pre class="lang:default decode:true ">git clone https://github.com/manthanhd/apache-mahout-recommendation-starter.git</pre>
Change to that directory using <span class="lang:default decode:true crayon-inline">cd</span>. Assuming you have <span class="lang:default decode:true crayon-inline ">gradle</span> installed, run:
<pre class="lang:default decode:true">gradle clean run</pre>
You should see output like:
<pre class="lang:default mark:10-12 decode:true">11:47:58: Executing external tasks 'clean run'...
:clean
:compileJava
:processResources
:classes
log4j:WARN No appenders could be found for logger (org.apache.mahout.cf.taste.impl.model.file.FileDataModel).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
:run
RecommendedItem[item:12, value:4.8328104]
RecommendedItem[item:13, value:4.6656213]
RecommendedItem[item:14, value:4.331242]

BUILD SUCCESSFUL

Total time: 0.52 secs
11:47:58: External tasks execution finished 'clean run'.</pre>
The highlighted lines are the recommendations that have been generated. The training data set for this can be found in <span class="lang:default decode:true crayon-inline ">src/main/resources/dataset.csv</span> file. You can also find it <a href="https://raw.githubusercontent.com/manthanhd/apache-mahout-recommendation-starter/master/src/main/resources/dataset.csv" target="_blank">here</a>.

From those highlighted lines, the <span class="lang:default decode:true crayon-inline">item:12</span>, <span class="lang:default decode:true crayon-inline ">item:13</span> and <span class="lang:default decode:true crayon-inline">item:14</span> refer to the <span class="lang:default decode:true crayon-inline ">ItemId</span> (second column in data set) and <span class="lang:default decode:true crayon-inline">value:4.83</span>, <span class="lang:default decode:true crayon-inline">value:4.66</span> and <span class="lang:default decode:true crayon-inline ">value:4.33</span> refer to the recommendation strength. This range depends on the implementation.
<pre class="lang:default mark:3 decode:true">public static void main(String[] args) throws TasteException {
    Starter starter = new Starter();
    starter.recommendFor(2, 3);
}</pre>
Looking at the code in <span class="lang:default decode:true crayon-inline">Starter.java</span> file, the recommendation is made for <span class="lang:default decode:true crayon-inline">UserId</span> (first column in data set) 2. By providing 3 as the second argument we're asking for 3 item recommendations for this user.

Well, so now that you have the basic framework in place, feel free to play with the <span class="lang:default decode:true crayon-inline ">UserId</span> values for recommendation, recommendation sizes or even with the data set itself. The more training data you add to the data set, the more accurate its recommendations will be.