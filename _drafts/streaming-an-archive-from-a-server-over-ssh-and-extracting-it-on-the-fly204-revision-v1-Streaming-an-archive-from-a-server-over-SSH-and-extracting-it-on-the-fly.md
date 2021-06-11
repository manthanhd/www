---
id: 205
title: Streaming an archive from a server over SSH and extracting it on the fly
date: 2015-08-26T18:20:52+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2015/08/26/204-revision-v1/
permalink: /?p=205
---
I'd found this command a while ago now but had completely forgotten about it until I stumbled upon it yesterday. So, if you had one or more tar files on a server and wanted to get them all down and then extract them somewhere to do your work, the normal way to achieve that would be:

<code>scp awesomeuser@awesomeserver:/tmp/mytarfile.tar /storage/</code>

The problem with this is that its a two step action. 1) You download the tar file(s). 2) You extract the downloaded tar file(s). While this is "functional", here's an awesome way to do it:<!--more-->

<code>curl -u awesomeuser: --key /home/awesomeuser/.ssh/id_rsa --pubkey /home/awesomeuser/.ssh/id_rsa.pub scp://awesomeserver/tmp/mytarfile.tar | tar xf -</code>

Lets break that down. We're using the curl command to download the file using <code>awesomeuser</code> as the username (indicated by <code>-u awesomeuser</code>). For authentication, we are using our private and public keys as indicated by <code>--key /home/awesomeuser/.ssh/id_rsa</code> and <code>--pubkey /home/awesomeuser/.ssh/id_rsa.pub</code> flags. We're then asking the curl command to use the <code>scp</code> protocol for accessing our server (as indicated by <code>scp://awesomeserver</code>) and are then giving it the location of the tar file that we want to download (as indicated by <code>/tmp/mytarfile.tar</code>). Because curl command will output whatever it gets to standard out stream (<code>STDOUT</code>), we're leveraging this capability by redirecting <code>STDOUT</code> to a pipe which is then fed into the tar command (as indicated by <code>| tar xf -</code>).

In case you have a <code>tar.gz</code> file or a <code>tgz</code> file, use:

<code>curl -u awesomeuser: --key /home/awesomeuser/.ssh/id_rsa --pubkey /home/awesomeuser/.ssh/id_rsa.pub scp://awesomeserver/tmp/mytarfile.tar.gz | gunzip -c - | tar xf -</code>

Same principle except before the final pipe of <code>tar xf -</code>, we're squeezing <code>gunzip -c</code> in between to g-unzip the file first before untarring it.