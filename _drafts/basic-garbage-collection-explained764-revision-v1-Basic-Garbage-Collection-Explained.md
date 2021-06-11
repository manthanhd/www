---
id: 875
title: Basic Garbage Collection Explained
date: 2020-09-14T11:52:45+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2020/09/14/764-revision-v1/
permalink: /?p=875
---
<!-- wp:image -->
<figure class="wp-block-image"><img src="https://s.inyourpocket.com/img/figure/2016-11/EB3.jpg" alt="Image result for english pub"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph {"dropCap":true} -->
<p class="has-drop-cap">Imagine you are inside a pub, having a few drinks with your mates. Eventually, as time passes, your table starts to fill up with empty glasses. You have two options to reduce the clutter: </p>
<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->
<ol><li>You clean up the table yourself by picking up the glasses and dropping them at the bar</li><li>You wait for one of the servers to come by and clean it up for you.</li></ol>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>There is a third option where you raise your neck like a meerkat and try to make eye contact with one of the servers, hoping for them to come by your table wherein you order more drinks and in the process they clean up the table for you. We'll discuss this option later.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Cleaning the glasses by yourself is hard but a sure way to clean things up. You know who's still drinking and which glasses are clear so you can be precise. However, it is hard work because you have to be constantly mindful of the utilisation and have to keep doing it.</p>
<!-- /wp:paragraph -->

<!-- wp:image -->
<figure class="wp-block-image"><img src="https://ichef.bbci.co.uk/news/660/cpsprodpb/9E04/production/_100525404_gettyimages-475705672-2.jpg" alt="Image result for english pub table drinks"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>There is a better way.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>If the pub you're at has servers, one of them can come by and clear up the glasses. However, this approach is less precise because while they make sure they only clear empty glasses, sometimes they misjudge and can accidentally clear up ones that have that some drink left. Also the frequency of the cleanup would have to be fine tuned - although experienced servers and staff have this nailed down pretty well. If they clear up too frequently (imagine a server grabbing the empty glass out of your hand as soon as you're done) it would be rude and would leave you out of a drink, but if they do it too late, glasses will pile up. Either extreme would result in annoyed patrons who will might leave to find another pub with better staff.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>Manual GC</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>When programming using languages that don't have "managed memory management", the former is the case. When the program runs, it will create variables and objects that take up the memory space. If the memory space is not "managed", the programmer has to make sure to put instructions in code to clear up the memory. This could be seen as a boon from performance perspective because the programmer would know when to and when not to clear the memory. Additionally, it gives programmer more control over what is running, i.e. there is no garbage collector process or thread present which could interfere with system's performance. </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>At the same time, this could be problematic because they might forget to clear some parts which could eventually fill up the memory over time. Also, going back to the pub analogy, if you're at the pub having a nice chat with your mates, it can get annoying and time consuming on your part to have to remember to clear up the table from time to time. If you forget and glasses keep piling up to a point when the table can no longer hold more glasses, adding one more glass onto the pile could make them all crash onto the floor, ruining everybody's evening. The program, in the same way could crash if the programmer either forgets to clear up some objects or references or clears up objects or references that are still in use.</p>
<!-- /wp:paragraph -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"><p>If the memory space is not "managed", the programmer has to make sure to put instructions in code to clear up the memory</p></blockquote>
<!-- /wp:quote -->

<!-- wp:paragraph -->
<p>Some languages, like Java for instance, have built in garbage collection. What this means is, instead of the programmer having to clear up the memory manually, there is a small, low priority thread running in the background that watches the lifecycle of the objects and variables that are created, making sure that unused ones get cleared up. This approach is great from programming perspective because as a programmer, I don't have to micro-manage the memory. It frees me up to do other things like writing elegant code and focusing more on the business logic. However, as we established earlier, this is not very precise because while the garbage collector is watching the objects and taking notes, it takes up a small, tiny amount of CPU power and memory. In the grand scale of things, this is small, however, arguably those resources could be better spent in processing more requests.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>Serial GC</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Now when it comes to clearing up the memory, the garbage collector can take different approaches. A simplistic approach is to simply freeze the program and clear up all objects and variables that are no longer referenced. While this is ok for small applications, it is not ideal because for that time, the program is unresponsive. Usually with small programs, this time is in micro or even nano seconds so it won't matter, however with large applications using gigabytes of memory, this time could be a couple of seconds. This is actually how garbage collection algorithm used to work in the old days and you can still use it now using the `-XX:+UseSerialGC` flag when running your Java application.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>If that wasn't clear, lets go back to the pub analogy. Having Serial GC is like having one server in the entire pub who goes around and clears up the unused glasses off of people's tables. But instead of doing it silently and courteously, they'll just climb up on top of table and shout "EVERYBODY FREEZE". Everybody freezes like a statue making it easy for them to go around and collect the empty glasses. When they're done, they'll shout "CONTINUE" and everybody would resume like nothing happened. Now, even if everybody is pretending, this could get annoying if the pub doesn't have sufficient glasses and have to call "FREEZE!" a lot (read low available memory) or if the pub is really busy and everybody is drinking really fast (read highly variable memory utilisation).</p>
<!-- /wp:paragraph -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"><p>A simplistic approach is to simply freeze the program and clear up all objects and variables that are no longer referenced.</p></blockquote>
<!-- /wp:quote -->

<!-- wp:paragraph -->
<p>This act of shouting "FREEZE!" and "CONTINUE!", in technical terms is called a Stop The World (STW) action. If required, JVM will call STW to pause all threads and objects from processing anything, resuming only when garbage collection is done.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>CMS GC</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Going back to the pub analogy, there is another way to clear up the tables of unused glasses. When the pub is busy, instead of picking up and carrying the glasses at once, the server will watch the tables and mentally mark the glasses that are empty. If the glasses are not touched on say the third mark, they are collected. Simple right? In terms of garbage collection in Java, this approach is called the Concurrent Mark and Sweep (CMS).&nbsp;</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>A CMS garbage collector has two primary functions. Mark and Sweep. Marking is action where references to an object are marked. When an object no longer has any references, it is "sweeped". CMS is a concurrent algorithm, taking full advantage of the modern multi core processors.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>G1 GC</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Like a couple of servers in a small pub, CMS works effectively upto a certain point. When heap sizes increase beyond 4GB, the efficiency that it brings starts to taper off. Relating back to the pub analogy, servers going around the whole pub marking and clearing glasses off the tables works efficiently up to a point. When the pub is too large say it has multiple levels or is large enough to hold say &gt; 2000 people, going around the whole pub becomes tiring for the servers.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This is where the G1 Garbage Collector (G1GC) comes in. The basic premise of it's algorithm is to divide up the heap space into sections such that each garbage collection thread has a small enough section to manage. Think of this as each server in a pub having its own dedicated area to manage, like it usually is in the real world! Here since each server is operating in their own area, it is not only easier to manage the objects but more importantly it reduces overall overhead. </p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>Conclusion</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Garbage collection is pretty interesting topic. There are several built in as well as custom algorithms available. The Java virtual machine allows for a lot of customisation not only in the garbage collection approach but also in tuning of the garbage collection algorithm itself. However, it is a bit of a black art in the sense that there is no silver bullet and each approach is unique to many things like the supporting hardware, application use case, application architecture, underlying system settings and features etc. Having said that though, it is important to note that there is no inherently bad garbage collection algorithm. Its all relative.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>For instance, while the default garbage collection algorithm is the concurrent mark sweep, it is a bad choice if you are running single core machine (although I think it switches from cms automatically if the number of CPUs==1) and if your application is sort of a time-driven batch application. In that case, the serial GC is perfectly a fine choice despite the STW freezes. Why? Well because the batch application can do its thing, using reasonable amount of memory and when its done, who cares if the application freezes? Again, use case is king here.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>In this post I've tried to simplify the concepts around garbage collection in Java to make it easy for everyone to understand. Understand that the choice of an optimal algorithm isn't as clear cut as it seems and it might take some effort to actually find the right GC algorithm for your application. I'll cover this some other time, but please do remember that even the best garbage collector algorithms are no replacement for bad code or bad architecture! Choosing a wrong algorithm could wreck the application performance as much as the right algorithm that could enhance it. Usually there are certain markers that should tell you if the garbage collector is working against you and this is the point at which you should start your analysis. Having said that though, the default CMS is perfectly acceptable in most cases and you probably don't need to spend two sprints analysing your application.</p>
<!-- /wp:paragraph -->