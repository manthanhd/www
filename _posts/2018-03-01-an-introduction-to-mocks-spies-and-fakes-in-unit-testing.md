---
id: 697
title: An introduction to Mocks, Spies and Fakes in unit testing
date: 2018-03-01T06:56:55+00:00
author: Manthan Dave
layout: post
guid: https://www.manthanhd.com/?p=697
permalink: /2018/03/01/an-introduction-to-mocks-spies-and-fakes-in-unit-testing/
categories:
  - Thoughts
tags:
  - tdd
  - testing
  - unit testing
---
When we’re unit testing, in addition to knowing the boundary conditions, it is important to know what classification our dependencies fall into, in testing terms. These classifications are:
<ul>
 	<li>Mocks</li>
 	<li>Spies</li>
 	<li>Fakes</li>
</ul>
<h2><strong>Mocks</strong></h2>
As the name suggests, a mock is a proxy or placeholder value that can be programmed to respond just like a real object would under any or matched conditions. A mock should be used where the real object dependency we have is very complicated to deal with during testing. This could be (but is not limited to):
<ul>
 	<li>Third party interactions (database/queue/api/files)</li>
 	<li>Classes that do really complicated stuff that take a lot of time</li>
 	<li>Things that work with random</li>
 	<li>Time based work</li>
</ul>
Mocking dependencies is a three phased approach:
<ol>
 	<li>Create the mock of the dependency based on the class or interface.</li>
 	<li>“Arm the mock” by preparing it work request/responses based on any or matched interactions</li>
 	<li>Set the mock on the test subject</li>
</ol>
Creating the mock could be the easiest or the hardest thing of all of the three because it depends on what you’re mocking and how you’re mocking it. If the test subject is relying on a class directly rather than an interface, mocking could become a bit trickier depending on how that class was implemented. This is because when you’re mocking a class, it has to first create a new instance of that class (by calling its constructor and by extension constructing all its ancestors). If at any one point, if one of the ancestors is doing something funky with one of its dependencies within the constructor, you’ll have to mock that out. This requires some level of knowledge or understanding of the entire class hierarchy and this is why use of interfaces for your dependencies is preferred.

When mocking an interface, Mockito uses JDK Proxy (as opposed to CGLib when mocking a class) which plays much more nicely with everything else. Also, because interfaces cannot be instantiated, there is no hierarchy to follow and the JDK Proxy wraps around it without any problems (or worry about dependencies). Although note that I have yet to personally test how this works with Java 9 which allows interfaces to have implementation code in them.

Arming the mock is relatively straight forward and involves three steps:
<ul>
 	<li>Knowing which part of class to “arm” <em>Which method are you mocking?</em></li>
 	<li>Knowing what to return (or do) <em>What should that method return/do?</em></li>
 	<li>Knowing what to match <em>Under which parameters should this mock work?</em></li>
</ul>
In most mocking frameworks, you can mock methods and have them either call the real thing (if its a spy; covered in next point) or have them return a custom value. In addition to this, you can specify the circumstances under which the mock should do what you’ve configured it to do. This is where the matchers come in. I am not going to cover how the matchers work in this post as that is a whole thing of its own, you can find out more here:

<a href="https://static.javadoc.io/org.mockito/mockito-all/1.9.0/org/mockito/Matchers.html">https://static.javadoc.io/org.mockito/mockito-all/1.9.0/org/mockito/Matchers.html</a>

Once you’ve armed the mock, it is time to make it available to the test subject. Now, ideally, this should be done in a way that the test subject isn’t even aware of the mocking, in a most non-intrusive way. Here are a couple of ways you can do this in decreasing order of intrusion:
<ul>
 	<li>Setter methods with default access modifier</li>
 	<li>Constructor args</li>
 	<li>Inversion of control - field level dependency Injection magic</li>
</ul>
Obviously its great to aim for point three but sometimes its harder to get DI involved in a project that has never has it so my suggestion here would be to work backwards from that.

When testing, while mocks are great at controlling the behaviours of the dependencies that your implementation is relying on, it is also great to verify whether or not the dependency is used in certain flows. In mockito for example, here you can ask Mockito to verify whether a particular mocked method was called, how many times it was called and even checking in certain cases to ensure that it wasn’t called!
<h2><strong>Spies</strong></h2>
Spies are like mocks, the only difference being that there is a real object under there. This is not the “real” object that CGLib wraps around, it is the one that you create to then set a spy on. Generally, spies are used in cases where you want to track the interactions that your test subject is making. This is nasty in my opinion and should only be used where you have to mock one of the test subject’s own methods (because they are being called from one of its other methods) or when you want to verify that a certain method is called.

Why is it nasty? Well it is because technically after setting a spy on the test subject object, it is no longer the real test subject. In addition to that spying on the test subject makes it easy to accidentally leave a mock on one of the methods of the test subject’s spy without realising and have the test(s) accidentally pass!

Spying generally involves three steps:
<ol>
 	<li>Create real object</li>
 	<li>Create Spy around the real object</li>
 	<li>Set (or use) the spy</li>
</ol>
<h2><strong>Fakes</strong></h2>
Faking is not mocking. Fakes are real objects but they have fake values in them. My suggestion here is to fake POJOs and other “simple” classes. The general rule of thumb that I think everybody should follow is that if the external dependency is more work to mock than fake then fake. Faking should extend, but not be limited to:
<ul>
 	<li>Data structures (Map, Queue, List)</li>
 	<li>POJOs</li>
 	<li>Builder classes</li>
</ul>
Like mocks, using a fake involves two simple steps:
<ol>
 	<li>Create the fake with required values</li>
 	<li>Set the fake</li>
</ol>
As always, be mindful of the inheritance hierarchy here to ensure it doesn’t become unreliable and make our lives harder than it should be. An unreliable fake is a marker for something that should really be mocked.

Of course, all the three classifications above can be used in conjunction with each other. For example, recently I had to spy on the test subject, mock one of its methods to return a mock object which then in turn returns a fake object when one of its methods is called!