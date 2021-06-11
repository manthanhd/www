---
id: 711
title: Update all pom versions at once using maven versions
date: 2018-03-08T11:01:16+00:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2018/03/08/708-revision-v1/
permalink: /?p=711
---
Most of the projects I work on are multi-module projects so updating versions of pom files manually is a bit pain. As always, here's a command you can run that will update all pom versions in one go:
<pre class="lang:sh decode:true ">mvn versions:set -DnewVersion=2.0-SNAPSHOT</pre>
Make sure all your modules are discoverable. You can do this by enabling all your profiles in case some of your sub-modules are not visible from the main pom.
<pre class="lang:sh decode:true">mvn -P profile1,profile2 versions:set -DnewVersion=2.0-SNAPSHOT</pre>
Source: <a href="https://stackoverflow.com/a/5726412">https://stackoverflow.com/a/5726412</a>