---
id: 739
title: Replace piece of text found using regular expression in all matching files in a folder
date: 2018-06-20T12:00:17+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2018/06/20/737-revision-v1/
permalink: /?p=739
---
&nbsp;
<pre class="lang:sh decode:true ">find . -type f -exec grep -lE "ToBeReplaced[\\d-]+" {} \; -print | uniq | xargs -n 1 -I {} sed -Ei '' 's/(ToBeReplaced[\\d-]+)/WithSomething/g' "{}"</pre>
&nbsp;

&nbsp;