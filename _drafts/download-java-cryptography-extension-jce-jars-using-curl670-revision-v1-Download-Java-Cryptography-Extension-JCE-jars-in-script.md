---
id: 673
title: Download Java Cryptography Extension (JCE) jars in script
date: 2017-09-03T20:09:55+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2017/09/03/670-revision-v1/
permalink: /?p=673
---
I can't remember the exact source but I found the following command to download Java Cryptography Extension jars.
<pre class="lang:sh decode:true">curl -q -L -C - -b "oraclelicense=accept-securebackup-cookie" -o /tmp/jce_policy-8.zip -O http://download.oracle.com/otn-pub/java/jce/8/jce_policy-8.zip \&lt;br&gt;    &amp;&amp; unzip -oj -d /usr/lib/jvm/java-8-openjdk-amd64/jre/lib/security /tmp/jce_policy-8.zip \*/\*.jar \&lt;br&gt;    &amp;&amp; rm /tmp/jce_policy-8.zip</pre>
At this point in time, I don't have links to other versions of this. I needed it for Java 8 and I found it for Java 8.