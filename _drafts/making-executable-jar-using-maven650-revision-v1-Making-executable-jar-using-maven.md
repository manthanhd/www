---
id: 655
title: Making executable jar using maven
date: 2017-08-17T09:11:22+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2017/08/17/650-revision-v1/
permalink: /?p=655
---
I was trying something out the other day and wanted to write a really simple application. So I created a simple application backed by Maven.

Now I could run the jar file that maven built using the standard <code>-e</code> flag that lets java know the entry point but all that is too main stream. I wanted maven to handle that for me.

After doing some googling, I found a plugin provided by codehaus. This one allowed me to run the application through maven. As you can see below, the configuration is quite simple.
<pre class="lang:default decode:true">&lt;build&gt;
..
  &lt;plugin&gt;
    &lt;groupId&gt;org.codehaus.mojo&lt;/groupId&gt;
    &lt;artifactId&gt;exec-maven-plugin&lt;/artifactId&gt;
    &lt;version&gt;1.2.1&lt;/version&gt;
    &lt;configuration&gt;
      &lt;mainClass&gt;com.manthanhd.cli.RunCli&lt;/mainClass&gt;
    &lt;/configuration&gt;
  &lt;/plugin&gt;
..
&lt;/build&gt;
</pre>
Make sure you update the value inside <code>mainClass</code> with fully qualified name of your class that you want to run. This class must have a <code>public static void main</code> method in it.

Once you are happy with the configuration, you can run your application by executing the following in your terminal
<pre class="lang:default decode:true">mvn exec:java</pre>
While this is great, I cannot run the jar on its own on a server somewhere. If I wanted to, I'd have to get the source code with maven and then run it using the above command. Thats sub-optimal. So I went googling again.

Finally, I found this wonderful plugin provided by our friends at Apache. This is the <code>maven-jar-plugin</code>. This is a standard jar plugin but one of its features is ability to specify a <code>mainClass</code> attribute - just like the codehaus plugin. But unlike the codehaus plugin, I can run the jar as a standalone application without needing to pass in any other flags or parameters indicating the main class.

Here's the maven build plugin configuration to use the <code>maven-jar-plugin</code>.
<pre class="lang:default decode:true">&lt;build&gt;
...
  &lt;plugin&gt;
    &lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
    &lt;artifactId&gt;maven-jar-plugin&lt;/artifactId&gt;
    &lt;configuration&gt;
      &lt;archive&gt;
          &lt;manifest&gt;
              &lt;addClasspath&gt;true&lt;/addClasspath&gt;
              &lt;mainClass&gt;com.manthanhd.cli.RunCli&lt;/mainClass&gt;
          &lt;/manifest&gt;
      &lt;/archive&gt;
    &lt;/configuration&gt;
  &lt;/plugin&gt;
...
&lt;/build&gt;</pre>
As it is standard with maven, package up your application using:
<pre class="lang:sh decode:true ">mvn clean install</pre>
If the build was successful, just run your jar file using standard <code>java -jar path/to/app.jar</code> command. In my case, I ran:
<pre class="lang:sh decode:true">java -jar ./target/my-awesome-standalone-app.jar</pre>
&nbsp;