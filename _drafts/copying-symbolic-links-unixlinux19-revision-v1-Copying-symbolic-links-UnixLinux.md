---
id: 181
title: Copying symbolic links (Unix/Linux)
date: 2015-08-25T23:18:18+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2015/08/25/19-revision-v1/
permalink: /?p=181
---
At some point, you might come across a situation where you have some files and a bunch of symlinks (symbolic links) in a folder and you wish to copy the data that is pointed at by the symlinks and not just blank links. If you are using Redhat Enterprise Linux, this should automatically happen when you call the cp command. However, on UNIX platforms or Linux distributions, this is not the case by default. In those cases, you will have to issue the following command:
<!--more-->

<code>cp -L {file(s)} {destination folder}</code>
<code>
</code>The -L option in cp command forces it to follow (dereference) symlinks and copy the data that is pointed at by the symlink.

In case you do not want to follow symlinks, you can issue the following command instead:
<code>
</code><code>cp -P {file(s)} {destination folder}</code><span style="font-family: monospace;"> </span>