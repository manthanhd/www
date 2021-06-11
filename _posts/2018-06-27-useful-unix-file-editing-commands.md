---
id: 737
title: Useful unix file editing commands
date: 2018-06-27T13:55:15+01:00
author: Manthan Dave
layout: post
guid: https://www.manthanhd.com/?p=737
permalink: /2018/06/27/useful-unix-file-editing-commands/
categories:
  - Educational
tags:
  - bash
  - linux
  - shell
  - unix
---
<h4>Find and replace text matching a regular expression in a single file</h4>
<pre class="lang:sh decode:true">sed -E -i '' 's/(something\\-[\\da-zA-Z]+)/ToReplace/g' path/to/file.txt</pre>
The above command searches for the <code>(something\-[\da-zA-Z]+)</code> regular expression and replaces it whole (because of the parenthesis which means to select the text matching the expression within) with <code>ToReplace</code>. The <code>g</code> in the end indicates that the operation will be applied to all matches in the file as supplied in <code>path/to/file.txt</code> argument. The <code>-i</code> parameter along with <code>''</code> suggests the <code>sed</code> command to perform the edit on the file itself, without creating a new copy.
<h4>Find and replace text matching a regular expression in files matching name</h4>
<pre class="lang:sh decode:true">find /base/directory -name "*.txt" -type f | xargs -n 1 sed -E -i '' 's/hello/hi/g'</pre>
The above command finds file within <code>/base/directory</code> whose names match <code>*.txt</code> format. Later we combine the output of this with <code>xargs</code> which appends each line of output (path to each matching file) to the following <code>sed</code> command. The <code>-n 1</code> argument to the <code>xargs</code> command tells it to supply each line of argument one by one to the <code>sed</code> command.
<h4>Find and replace text matching a regular expression in files whose contents match a regular expression</h4>
<pre class="lang:sh decode:true">grep -rlE 'TextToFindInFiles' $(find /base/directory -name '*.txt' -type f) | xargs -n 1 sed -E -i '' 's/hi/hello/g'</pre>
In the above command we use the basic <code>find</code> command to get a list of files that we want to do the search in. Then we use <code>grep</code> to recursively find in those files. Now in the above command we don't really need the <code>-r</code> flag for grep because the <code>find</code> command will list full paths to those files, but we would need it if we were doing find on a relative path instead (like <code>.</code>). The <code>-l</code> flag for grep here will only list the paths to the files that it found having the content <code>TextToFindInFiles</code> and not the actual matching contents like it usually prints. This list of the files is then outputted to the <code>xargs</code> command which then subsequently runs the <code>sed</code> command.

&nbsp;