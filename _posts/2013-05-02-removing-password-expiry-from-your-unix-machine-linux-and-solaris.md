---
id: 27
title: Removing password expiry from your Linux/Unix machine
date: 2013-05-02T17:47:00+01:00
author: Manthan Dave
layout: post
guid: http://www.manthanhd.com/2013/05/02/removing-password-expiry-from-your-unix-machine-linux-and-solaris/
permalink: /2013/05/02/removing-password-expiry-from-your-unix-machine-linux-and-solaris/
blogger_blog:
  - codeninjutsu.blogspot.com
blogger_author:
  - ManthanHD
blogger_permalink:
  - /2013/05/removing-password-expiry-from-your-unix.html
blogger_internal:
  - /feeds/2026358850785924011/posts/default/5424482383122772680
categories:
  - Findings
tags:
  - linux.solaris
  - shell
  - unix
---
Here's a quick one. If you have a server and you do not want the password for a user to expire (as it can screw some things up while its active), you need to execute the following commands as <b><code>root</code></b>:
<code>passwd -x -1 </code>

where is the username whose password expiry you wish to remove. For instance, in my case, if username is dm014, I executed:
<code>passwd -x -1 dm014</code>
<code>
</code> I have tested this and it works flawlessly on most Linux and Solaris operating systems.