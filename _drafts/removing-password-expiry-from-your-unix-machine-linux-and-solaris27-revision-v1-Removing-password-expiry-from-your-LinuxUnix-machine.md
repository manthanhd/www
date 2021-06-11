---
id: 176
title: Removing password expiry from your Linux/Unix machine
date: 2015-08-25T23:14:01+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2015/08/25/27-revision-v1/
permalink: /?p=176
---
Here's a quick one. If you have a server and you do not want the password for a user to expire (as it can screw some things up while its active), you need to execute the following commands as <b><code>root</code></b>:
<code>passwd -x -1 </code>

where is the username whose password expiry you wish to remove. For instance, in my case, if username is dm014, I executed:
<code>passwd -x -1 dm014</code>
<code>
</code> I have tested this and it works flawlessly on most Linux and Solaris operating systems.