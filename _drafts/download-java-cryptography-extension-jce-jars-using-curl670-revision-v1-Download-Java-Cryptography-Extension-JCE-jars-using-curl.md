---
id: 688
title: Download Java Cryptography Extension (JCE) jars using curl
date: 2017-12-24T10:59:11+00:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2017/12/24/670-revision-v1/
permalink: /?p=688
---
Normally, in order to acquire the JCE jars, you have to:
<ol>
 	<li>Go to google</li>
 	<li>Search JCE Jars</li>
 	<li>Navigate to oracle website from search results</li>
 	<li>Accept some agreement agreeing to sell your soul to Oracle</li>
 	<li>Finally download the jars.</li>
</ol>
While this is great, it doesn't work well for when you want JCE jars in your docker container. I mean, yeah you can get it by downloading it into a static directory so that it is available to docker during build time but to be honest, that is a bit lame. Lamer than just fetching it from a URL.

Now, I can't remember the exact source but after much head banging and googling, I found the following command to download Java Cryptography Extension jars on the fly using curl.
<pre class="lang:sh decode:true">curl -q -L -C - -b "oraclelicense=accept-securebackup-cookie" -o /tmp/jce_policy-8.zip -O http://download.oracle.com/otn-pub/java/jce/8/jce_policy-8.zip \&lt;br&gt;    &amp;&amp; unzip -oj -d /usr/lib/jvm/java-8-openjdk-amd64/jre/lib/security /tmp/jce_policy-8.zip \*/\*.jar \&lt;br&gt;    &amp;&amp; rm /tmp/jce_policy-8.zip</pre>
At this point in time, I don't have links to other versions of this. I needed it for Java 8 and I found it for Java 8. Regardless, I can't imagine why it wouldn't work if you'd just change the version from 8 to 7 in the above command.