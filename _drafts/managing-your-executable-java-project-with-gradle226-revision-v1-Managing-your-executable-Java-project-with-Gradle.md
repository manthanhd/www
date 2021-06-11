---
id: 232
title: Managing your executable Java project with Gradle
date: 2015-08-31T13:23:49+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2015/08/31/226-revision-v1/
permalink: /?p=232
---
I've been using Gradle for a while now and have quite used to it. Most of my Java projects are web applications so all Gradle has to do is to build them. Once built, I can just plop the war file onto tomcat and it just works. However, when I'm trying something out, I just need a simple .java file which I manually compile with <code>javac</code> and then run it with <code>java</code> command. This works most of the time, except when I wanted to try out something complex, something that uses loads of external libaries. Something like this normally needs a dependency management system like Gradle. Using my normal Gradle configuration, I got it to build the project really easily, however running it was quite difficult.<!--more-->

The main difficulty came from classpath problems. When you are running a web application via tomcat, it usually takes care of all that stuff. However, when you're doing it manually, YOU have to take care of it. Again, because this required manual effort, I started finding ways to achieve that via Gradle. After a couple of hours, I finally got it working with the following build.gradle file:
<pre class="lang:groovy ">apply plugin: 'java'
apply plugin: 'idea'
apply plugin: 'application'

mainClassName = "com.mypackage.awesomeapp.Main"

dependencies {
compile 'org.apache.httpcomponents:httpclient:4.5'
}

run {
systemProperties System.getProperties()
println(systemProperties)
}
</pre>
The two key things here are the <code>apply plugin: 'application'</code> and <code>mainClassName = "com.mypackage.awesomeapp.Main"</code> lines. As the statement suggests, the first one applies the application plugin, declaring your project as a standalone application. The second one tells Gradle what class is your main class. Gradle then sets this in the manifest file of the resulting jar file so that when executed, it knows what class to invoke first. Once you have the setup ready, execute:

<code>gradle run</code>

Feel free to give me a shout if you find better solution than this and I'll make sure the article gets updated with proper credit. Enjoy!