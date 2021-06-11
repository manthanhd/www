---
id: 376
title: Clearing passwords on Linux
date: 2016-05-13T13:21:13+01:00
author: Manthan Dave
layout: post
guid: https://www.manthanhd.com/?p=376
permalink: /2016/05/13/clearing-passwords-on-linux/
categories:
  - Educational
tags:
  - linux
  - unix
---
As <span class="lang:sh decode:true crayon-inline ">root</span>, quickest way is to use the <span class="lang:default decode:true crayon-inline ">passwd</span>  command:
<pre class="lang:sh decode:true">passwd -d &lt;user&gt;</pre>
If there are failed attempts, you'd need to reset those as well:
<pre class="lang:sh decode:true">pam_tally2 --user=&lt;user&gt; --reset</pre>
To verify:
<pre class="lang:sh decode:true">pam_tally2 --user=&lt;user&gt;</pre>