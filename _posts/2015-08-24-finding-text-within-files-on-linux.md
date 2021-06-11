---
id: 116
title: Finding text within files on Linux
date: 2015-08-24T20:02:55+01:00
author: Manthan Dave
layout: post
guid: http://www.manthanhd.com/?p=116
permalink: /2015/08/24/finding-text-within-files-on-linux/
categories:
  - Findings
tags:
  - linux
  - shell
  - ubuntu
  - unix
---
So at some point in your professional life, you have probably faced the following problem:
<blockquote>I have a bunch of files and I want to quickly find a file that contains a particular text. I don't want to go through each file individually because that's lame. Also, I don't want to use GUI because I aspire to become pro at using Linux commands.</blockquote>
Yeah, yeah we've all been there. As a beginner, I struggled a lot with this and even if I found a solution, I won't be able to recall it the next time when I'd so desperately need it. Well, here it is!<!--more-->

If you are looking in all files whose name ends with <code>.txt</code>:

<code>find /home/manthan/folder_o_files -name *.txt -type f -exec grep awesome {} \; -print;</code>

In the above command, we're looking through all the files (denoted by <code>-type f</code>) in <code>/home/manthan/folder_o_files</code> folder. The <code>-exec</code> flag allows user to specify what command to execute for line of output from the find command. In this case, we're using our favourite command <code>grep</code> to find for text <code>awesome</code>. The opening and closing curly brackets (denoted by <code>{}</code>) serve as placeholder for each line of file outputted by the find command. We escape the semi-colon to end the grep command, ask find command to show its findings by supplying it with <code>-print</code> flag and then terminate our command by ending it with semi-colon.

If you are unsure of the case of the text (i.e. upper-case or lower-case), stick <code>-i</code> flag right after <code>grep</code>.

If for some reason you are looking through a bunch of tar files and are looking for a file that one of the tar files might contain, check out my other post on <strong><em>How to find a file contained with a bunch of tar files without opening</em></strong><em> (soon)</em>.

Enjoy!