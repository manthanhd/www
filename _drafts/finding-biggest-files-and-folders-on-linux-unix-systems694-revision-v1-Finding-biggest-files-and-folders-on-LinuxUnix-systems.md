---
id: 696
title: Finding biggest files and folders on Linux/Unix systems
date: 2018-02-23T09:31:16+00:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2018/02/23/694-revision-v1/
permalink: /?p=696
---
I was happily doing some builds on my Jenkins slave server at home and then suddenly boom! it broke. It took all the queued up builds with it because they all started failing. When I looked, it was a disk space issue.

Normally my builds don't take up much disk space so I started investigating. I needed to find files that were occupying largest size on the disk. After some trial and error, following command came to rescue:
<pre class="lang:sh decode:true ">find . -type f -exec du -sh {} \; -print | sort -n</pre>
This was great, but then I wanted to find folders that were the biggest. No problem!
<pre class="lang:default decode:true ">find . -type d -exec du -sh {} \; -print | sort -n</pre>
Notice the <code>-type d</code> above which differentiates it from the first command. Also note that the above command outputting the disk use by folder also includes subfolders, so generally the largest ones will be the top level folders and as you go up the list (its ascending in order by default) you'll find the subfolders with their sizes. You can always pipe the whole thing through grep to only look for the folders you want like so:
<pre class="lang:sh decode:true ">find . -type f -exec du -sh {} \; -print | sort -n | grep -i /target</pre>
While this is great and all, sometimes you just want to go upto certain depth within the file tree. Say no more!
<pre class="lang:sh decode:true ">find . -type f -maxdepth 2 -exec du -sh {} \; -print | sort -n | grep -i /target</pre>
Notice the `-maxdepth 2` flag which sets the max depth to 2.

However, you could be one of those people who don't like the find command at all. Maybe a past feud, or just a dislike. Well, the <code>du</code> command has your back!
<pre class="lang:sh decode:true">du -m -d 2 * --all | sort -n</pre>
The <code>-m</code> flag makes it print file sizes in megabytes, <code>-d 2</code> sets max depth to 2 and <code>--all</code> tells it to work with files as well as directories. Its actually quite comprehensive because using some clever flags like <code>-I</code> to provide a mask for files and directories to ignore and <code>-L</code> to follow symbolic links (they are not followed by default) you can get quite a lot out of it. Also, a quick note before ending this post, you can switch the file size block from <code>-m</code> for megabytes (example above) to <code>-g</code> for gigabytes or even <code>-k</code> for kilobytes. You can use <code>-h</code> for human readable where it will automatically choose the closest block size but this will confuse the sort because it doesn't quite take the size character in account and only sorts things using the numeric values.

Above commands have been tested on mac where they were installed as part of <a href="http://brewformulas.org/Coreutil">GNU CoreUtils homebrew package</a>.