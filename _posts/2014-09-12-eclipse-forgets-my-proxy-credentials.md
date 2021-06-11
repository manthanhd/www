---
id: 8
title: Fixing proxy credentials dementia in Eclipse
date: 2014-09-12T10:49:00+01:00
author: Manthan Dave
layout: post
permalink: /2014/09/12/eclipse-forgets-my-proxy-credentials/
blogger_blog:
  - codeninjutsu.blogspot.com
blogger_author:
  - ManthanHD
blogger_permalink:
  - /2014/09/eclipse-forgets-my-proxy-credentials.html
blogger_internal:
  - /feeds/2026358850785924011/posts/default/1765337601120678711
geo_latitude:
  - "55.378051"
geo_longitude:
  - "-3.435973"
geo_public:
  - "1"
geo_address:
  - United Kingdom
categories:
  - Findings
tags:
  - eclipse
  - troubleshooting
---
This usually happens when you are using a different eclipse installation to access your old eclipse workspace. Its something to do with the new installation not being able to access the secure storage created by your old installation. Here's what I did to solve it:

Close eclipse completely. Now fire up terminal or command prompt or whatever and navigate to your home directory. In my case it is <code>'/Users/manthan/'</code> directory.
<!--more-->
Now cd into <code>.eclipse/org.eclipse.equinox.security directory</code>.
<blockquote><code>cd .eclipse/org.eclipse.equinox.security</code></blockquote>
If you list files, you should see a file called secure_storage in it. Rename that to <code>secure_storage.old</code> or something like that. You can delete it if you want but I prefer to rename things, make sure it works and then delete the renamed file.
<div>
<blockquote><code>mv secure_storage secure_storage.old</code></blockquote>
</div>
<div>Now making sure that the <code>secure_storage</code> file has been renamed successfully, open eclipse and now try saving your credentials. It should ask for your key store master password and other stuff.</div>
<div></div>
<div>That's it! You're done.</div>