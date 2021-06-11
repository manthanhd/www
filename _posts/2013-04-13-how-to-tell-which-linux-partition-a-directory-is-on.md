---
id: 32
title: How to tell which Linux partition a directory is on?
date: 2013-04-13T23:01:00+01:00
author: Manthan Dave
layout: post
guid: http://www.manthanhd.com/2013/04/13/how-to-tell-which-linux-partition-a-directory-is-on/
permalink: /2013/04/13/how-to-tell-which-linux-partition-a-directory-is-on/
blogger_blog:
  - codeninjutsu.blogspot.com
blogger_author:
  - ManthanHD
blogger_permalink:
  - /2013/04/how-to-tell-which-linux-partition.html
blogger_internal:
  - /feeds/2026358850785924011/posts/default/1566988666465831723
categories:
  - Findings
tags:
  - linux
  - shell
---
This is really simple. First of all, you need to know the full path to that directory. "cd" into that directory and type:
<code>pwd
</code>
and it will display full path of the directory you are in. Remember that or even better, copy it to the clipboard. Type the following command to know the name of the partition:
<code>df
</code>
where replace with the full path of your directory. This in my case becomes "<code>df /data/android/sample</code>".

If you are already in the directory, you can type "<code>df .</code>" and it will give details about the partition you are currently on.

The output should be something like:
<pre style="background-image: none; border: 0px none white; font-family: monospace, monospace; font-size: 13px; line-height: 1.2em; padding: 0px; vertical-align: top;"><span style="background-color: #f9f9f9;">Filesystem    </span><span style="background-color: #f9f9f9;">1024</span><span style="background-color: #f9f9f9;">-blocks      Free </span><span style="background-color: #f9f9f9; font-weight: bold;">%</span><span style="background-color: #f9f9f9;">Used    Iused </span><span style="background-color: #f9f9f9; font-weight: bold;">%</span><span style="background-color: #f9f9f9;">Iused Mounted on
 </span><span style="background-color: #bf9000;"><span style="font-weight: bold;">/</span>dev<span style="font-weight: bold;">/</span>hd4</span>            <span style="background-color: #f9f9f9;">32768</span>     <span style="background-color: #f9f9f9;">16016</span>   <span style="background-color: #f9f9f9;">52</span><span style="background-color: #f9f9f9; font-weight: bold;">%</span>     <span style="background-color: #f9f9f9;">2271</span>    <span style="background-color: #f9f9f9;">14</span><span style="background-color: #f9f9f9; font-weight: bold;">%</span> <span style="background-color: #f9f9f9; font-weight: bold;">/</span></pre>
The highlighted part is the name of your partition.