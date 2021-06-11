---
id: 132
title: How to get file sizes recursively in a directory and list them in ascending order
date: 2015-08-24T22:47:46+01:00
author: Manthan Dave
layout: revision
guid: http://www.manthanhd.com/2015/08/24/6-autosave-v1/
permalink: /?p=132
---
When cleaning up unused files from my linux computer, I find it useful to know which files use most space so that I can easily determine which ones to remove.

The most basic command to find file size of a given file is:
<code>du -sm myfile.txt</code>
<!--more-->
If you are in a folder with multiple files and want to get file sizes of all the files, the command is:
<code>du -sm *</code>

The above command does not get file sizes recursively. However, if you are happy with it and want to sort the file sizes in ascending order:
<code>du -sm * | sort -n -k 1</code>

Now, this is little useful as it does not do recursive file size listing. To do that:
<code>find . -type f -exec du -sm {} \; | sort -n -k 1</code>

If you are looking for a particular type of file (eg. jar files):
<code>find . -name "*.jar" -type f -exec du -sm {} \; | sort -n -k 1</code>

Enjoy!