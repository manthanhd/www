---
id: 378
title: Clearing passwords on Linux
date: 2016-05-13T13:27:44+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2016/05/13/376-revision-v1/
permalink: /?p=378
---
As <span class="lang:sh decode:true  crayon-inline ">root</span>, quickest way is to use the <span class="lang:default decode:true crayon-inline ">passwd</span>  command:
<pre class="lang:sh decode:true">passwd -d &lt;user&gt;</pre>
If there are failed attempts, you'd need to reset those as well:
<pre class="lang:sh decode:true">pam_tally2 --user=&lt;user&gt; --reset</pre>
To verify:
<pre class="lang:sh decode:true">pam_tally2 --user=&lt;user&gt;</pre>