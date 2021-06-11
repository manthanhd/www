---
id: 528
title: Anatomy of a chat bot
date: 2016-12-12T15:40:34+00:00
author: Manthan Dave
layout: post
guid: https://www.manthanhd.com/?p=528
permalink: /?p=528
categories:
  - Thoughts
tags:
  - bot
  - chat
---
A basic chat bot consists of four components:
<ol>
 	<li>Channel Interface</li>
 	<li>Context Resolution</li>
 	<li>Classification</li>
 	<li>Skill Resolution</li>
</ol>
Lets go through them one by one.
<h2>Channel Interface</h2>
Simply put, a channel interface is a point of interface used to interact with the bot. Since chat bots use text as an input, this could be anything from a simple command line interface to something more complex like Facebook messenger, slack, skype etc.

A channel interface must be able to provide two things. Input message in text as well as a correspondence ID. The input message, without any doubt is the textual natural language input entered by the user. The correspondence ID on the other hand is the ID of the entire interaction that is happening between the chat bot and the user. Think of this like a user identifier but instead of being linked to the entirely of the user, is only linked to the correspondence that the user is having with the chat bot.

This is not a session identifier either! As sessions can be created and destroyed. A correspondence is something that outlives a user session as for the bot to functional correctly, it must remember all interactions that it has had with the user since the first conversation. This, of course is subject to change as your use case might require the bot to forget all previous interactions when the user closes the window, in which case, the correspondence ID becomes the same as a session ID.

So to summarise, a channel interface is a point of interaction that allows a user to interface with the chat bot. This interface, must supply two things: textual input and correspondence ID for that input.
<h2>Context Resolution</h2>
Now this is where things start to get interesting. Being aware of the context when responding is one of the better traits of a good chat bot. There are two types of contexts:
<ol>
 	<li>Explicit</li>
 	<li>Implicit</li>
</ol>
Explicit context is where context is given within a supplied query. Lets look at the following statement:
<blockquote>What is the balance on my personal credit card?</blockquote>
The above statement provides explicit context containing four attributes, namely, object: credit card, name: personal, attribute: balance.

Explicit context helps you establish context from a sentence. Once such a context has been set, the user shouldn't need to specify the same context again and again. This is where the sweet sweet implicit context comes in.

Implicit context is context that has been set from previous interactions with the user. In other terms, when context is missing in a given query, it is derived from previous interactions. Say for instance the user has already said the above statement. That sets the context as follows:
<pre class="lang:default decode:true">{
    "object": "creditcard",
    "name": "personal",
    "attribute": "balance"
}</pre>
Now if the user says the following:
<blockquote>And remind me my limit on that?</blockquote>
With explicit context, the bot has no way of knowing what the customer is talking about except the attribute being limt. This is where the implicit context kicks in. From the previous conversation, it assumes that the conversation is still about the credit card, specifically the personal one. Using the explicit context derivation, it knows that the attribute this time is "limit". Viola! It should respond with the limit on your personal credit card without you specifying it explicitly!
<h2>Classification</h2>
You've been given a piece of text. How do you know what topic it is for? How do you classify it into a topic? This is where a classifier comes in. A classifier takes in an input, analyses it and classifies it into a topic. Sounds like magic right? Well, it kinda is.

Most classifiers have two kinds of input. Training and query. Training, as the name suggests is training data. It must comprise of two things. Query and a topic that the query classifies to. Consider the following example:
<blockquote>Tell me about an apple.</blockquote>
You and I know that the sentence above is about an apple. Thus, here the topic is apple. When you train a classifier, you'll have to provide the above sentence (or merely 'about apple' as query should suffice most classifiers) as well as the topic that we know as 'apple'. Obviously one is not enough so you'll have to make up as much training data as you can. These are known values.

The second kind of input is the query itself. This is where you genuinely don't know the topic for a given sentence and are asking the classifier to do the classification for you.

The most basic type of classification is logistic regression where for every line of text in training data set for a given topic, you extract features out of it, and then plot it on a graph. You then derive a line of regression for every topic. Whenever a given text input matches closest to one of the lines of regression, you return the topic for that regression line as "classified" topic. All the classifiers that I have seen so far provide two values when queried with a piece of text. First is the topic that the given piece of text has been classified to. Second is the level of confidence the classifier has in that classification. This normally ranges between 0 and 1 in decimals.
<h2>Skill Resolution</h2>
Until now, we've accepted input from a channel, resolved the context and classified the text into a topic. The next thing to do for the bot is to take action on it. This is where skill resolution comes in. However, before we go into skill resolution, lets briefly cover some details about the skill itself.

A skill is a piece of logic that can be executed in order to generate a response. This response can be a textual or in some cases (like Facebook messenger) html. Typically, a skill, when executed, must generate a response. In JavaScript terms, a skill is a simple object with an executable function. In Java, it could be an object that is an instance of a class implementing some Skill interface. Either way, there should be an explicit contract between the skill and the bot that states the callable logic that can be executed.

There are different levels of complexities associated with skill resolution. This could be something like "What happens when no skill is found for a given topic?", "How do I map skills according to the confidence in topic provided by the classifier?", "What happens when a skill doesn't respond at all?", "How do we manage requested responses from skills?" etc.