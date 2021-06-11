---
id: 120
title: Finding a file contained within a bunch of tar files without opening any
date: 2015-08-24T20:22:25+01:00
author: Manthan Dave
layout: revision
guid: http://www.manthanhd.com/2015/08/24/118-revision-v1/
permalink: /?p=120
---
So I was debugging some code the other day and I quickly found myself in a situation where I had to look for a class file that was located in one of the many millions of jar files that are there in my maven cache. My immediate reaction was
<blockquote>"Ugh, this is going to be loads of effort and is going to take ages!".</blockquote>
Luckily, quick one minute google found the following command:<!--more-->

<code>find <strong>/home/manthan/.m2</strong> -name '<strong>*.jar</strong>' -type f|while read f; do p="$(tar tf $f|egrep <strong>awesome</strong>)"; [ -n "$p" ] &amp;&amp; echo -e "$f\n$p" ; p=""; done</code>

Where <code>awesome.class</code> is the file I'm looking for in all files (indicated with <code>-type f</code> option) with extension <code>jar </code>found in <code>/home/manthan/.m2</code> directory.

Source: <a href="http://superuser.com/questions/838143/search-through-tar-files-for-a-pattern-and-print-the-full-path-of-whats-found" target="_blank">Superuser - Search through tar files for a pattern and print the full path of what's found</a>