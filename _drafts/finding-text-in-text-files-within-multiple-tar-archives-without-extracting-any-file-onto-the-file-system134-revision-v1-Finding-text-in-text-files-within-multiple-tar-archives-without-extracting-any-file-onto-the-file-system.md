---
id: 136
title: Finding text in text files within multiple tar archives without extracting any file onto the file system
date: 2015-08-25T19:07:02+01:00
author: Manthan Dave
layout: revision
guid: http://www.manthanhd.com/2015/08/25/134-revision-v1/
permalink: /?p=136
---
So I was working the other day and then suddenly I had a need to find a piece of text within loads of text files contained across multitude of tar files. In a normal case, you'd extract them all somewhere in temp directory and then use grep to find the text you want. However, this doesn't work very well when the tar files you have are in thousands. Also, there is something uncool about doing stuff manually when you're in a world where there's always a better way.

After an hour of experimentation, I found the following magic command:<!--more-->

<code>find /home/manthan/loads_of_tarfiles -name "*.tar" -type f -exec sh -c "tar -tf {} | grep awesome | xargs -I found -n 1 tar -Oxf {} found | grep im" \; -print</code>

The above command finds all files (indicated by <code>-type f</code>) in <code>/home/manthan/loads_of_tarfiles</code> folder with extension <code>.tar</code> (indicatetd by <code>-name "*.tar"</code>). For each file it finds, it runs a shell command (indicated by <code>-exec sh -c</code>) which lists all files contained within that tar file (indicated by <code>tar -tf {}</code>, where <code>{}</code> is each instance in which the find command found a tar file). For each file contained within the resident tar file, it then finds a text file whose name contains the word awesome (<code>grep awesome</code>). This can be anything as long as you know what kind of file you are looking for.

For each text file (whose name contains the word awesome) that is found within the tar file, it then extracts the contents of that file to <code>STDOUT</code> (indicated by <code>tar -Oxf {}</code> found where <code>{}</code> is the tar file that is found in the first part of the command and found is the text file that is found in the latter part of the command by the <code>grep awesome</code> command). Now that the contents of the file are available in <code>STDOUT</code>, a simple search finds the text we need in that file (using <code>grep im</code> which searches for text <code>im</code> within the extracted file). If it is found, the matching contents are printed and the name of the tar file it matched in is also printed (indicated by <code>-print</code>).

Phew! That was lengthy. As usual, if you have any questions or find a better way, let me know in the comments below and I'll make sure to update the post with credits. Enjoy!