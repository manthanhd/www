---
id: 564
title: Classification graphs for advanced natural language processing for chat bots
date: 2017-02-14T19:59:40+00:00
author: Manthan Dave
layout: post
guid: https://www.manthanhd.com/?p=564
permalink: /?p=564
categories:
  - Uncategorised
---
When I started working on chat bots, I had a rather simplistic view of the world. This included how I viewed processing of natural language might work. Originally, I pictured that one might write a single awesomemagic classifier that could classify a given sentence into literally everything. According to this view, I pictured that a single classifier might be able to classify a given sentence or query into the following:
<ol>
 	<li>check_temperature</li>
 	<li>identify_location</li>
 	<li>tell_joke</li>
 	<li>check_balance</li>
 	<li>tell_cat_facts</li>
</ol>
Now, for the above case, this would work wonderfully well. Looking back now, this was a clear illusion. The above example would work with a good classifier with average level of accuracy for most people, only because those topics that the classifier is classifying the input into are quite broad and disconnected from one another. Checking balance is quite far from asking the bot to tell cat facts. An input for identifying location is very different from checking temperature.

However, when you are trying to write a bot in a real world scenario where the topics are mostly quite closely linked to one another, my original logic starts to break down. This is because classifying inputs that are quite similar to one another becomes more difficult as you add more and more topics to classify similar input.

So, in short, this is a scalability issue because a single classifier cannot handle so much responsibility. A solution, from an engineering viewpoint is simple. Have many small, rather simplistic classifiers that classify one or two things. Then, connect them with one another such that the input can flow through.

The following diagram will be able to help visualise how this might work:

<a href="https://www.manthanhd.com/wp-content/uploads/2017/02/additive-classification.png"><img class="aligncenter size-large wp-image-565" src="https://www.manthanhd.com/wp-content/uploads/2017/02/additive-classification-700x459.png" alt="" width="700" height="459" /></a>

The yellow circles are classifiers while the blue dotted circles represent the classified output.

The way it works is this. The top level classifiers classify a given piece of input into high level domains. Depending on the output, the next classifier in that chain gets executed. All the output from across all the classifiers is captured within the request object which is then sent to the skill.

This differs quite a bit from the traditional way of resolving a topic to a skill which is currently to pick the topic with highest confidence from a list of topics provided from the classifiers. This results in a singular output from multiple inputs (multiple topics classified with different confidence levels across multiple classifiers into a single topic with highest confidence which is then resolved into a skill).

In the new way of doing things, this works by using clever matchers. This is because instead of a single output from multiple inputs, there'll be multiple outputs in the form of features.

# Feature Extraction

So far, all of this might be slightly overwhelming so lets distil some of the things down into a single concept so that we can better understand the whole picture.

Given a sentence, a chat bot needs to understand it.