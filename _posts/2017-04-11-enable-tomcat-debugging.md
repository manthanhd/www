---
id: 588
title: Enable tomcat debugging
date: 2017-04-11T07:25:51+01:00
author: Manthan Dave
layout: post
guid: https://www.manthanhd.com/?p=588
permalink: /2017/04/11/enable-tomcat-debugging/
categories:
  - Educational
  - Findings
tags:
  - debug
  - java
  - tomcat
---
Shutdown tomcat and make sure that it has properly shut down by monitoring the tomcat process.

Once it has completely shutdown, export the following environment variables:
<pre class="lang:sh decode:true ">export JPDA_ADDRESS=8000
export JPDA_TRANSPORT=dt_socket</pre>
Here, we're setting the jpda address to port <span class="lang:default decode:true crayon-inline">8000</span>. Note this down as you will need this port number in order to connect via your IDE or whatever debugging tool you're using.

Options above will run tomcat as usual so if you want to debug something that happens early on in the lifecycle, you either need to be really quick about attaching your debugger or add the following JPDA option:
<pre class="lang:sh decode:true">export JPDA_SUSPEND=y</pre>
This will suspend the tomcat startup until you attach your debugger.

Next, start tomcat jpda using catalina script:
<pre class="lang:sh decode:true">./catalina.sh jpda start</pre>
At this stage, you can monitor the log in <span class="lang:default decode:true crayon-inline ">catalina.out</span> file to make sure everything goes smoothly.

Now setup your IDE to connect to tomcat at port <span class="lang:default decode:true crayon-inline">8000</span>. If you are using vagrant as your VM, make sure you are port forwarding <span class="lang:default decode:true crayon-inline ">8000</span> in your network settings. If not, you can always tunnel in:
<pre class="lang:sh decode:true">vagrant ssh -- -L 8000:localhost:8000</pre>
Or if its a remote server, you can just use the normal ssh command:
<pre class="lang:sh decode:true crayon-selected">ssh -L 8000:localhost:8000 user@server.com</pre>
Hope this helps!

<strong>Credits</strong>
<ul>
 	<li>Conor Restall: For suggesting JPDA_SUSPEND option.</li>
</ul>