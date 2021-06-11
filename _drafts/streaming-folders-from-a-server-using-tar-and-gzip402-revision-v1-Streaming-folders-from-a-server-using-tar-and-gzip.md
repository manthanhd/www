---
id: 406
title: Streaming folders from a server using tar and gzip
date: 2016-08-22T23:09:35+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2016/08/22/402-revision-v1/
permalink: /?p=406
---
This is how you can stream a folder or multiple folders from a server over SSH protocol.

To stream folders in uncompressed, raw format, run:
<pre class="lang:sh decode:true ">ssh ec2-user@xx.xxx.xx.xxx "sudo tar -cO /folder1 /opt/folder2" | tar -xf -</pre>
Since the tar file is being streamed raw, this could potentially take longer as more data is being passed between server and client. In my test for 700 megabytes of data, this command took 1:56 seconds.

There's another method. This streams the tar file, pipes it into gzip on the remote server which comes out as compressed gzipped stream down to the client. The client then passes this compressed stream into gunzip which decompresses the stream and finally then pipes it into tar to extract it into its original folders.
<pre class="lang:sh decode:true ">ssh -i ~/.ssh/lao345-sandbox.pem ec2-user@10.122.64.165 "sudo tar -cO /prod/msp /opt/beasys | gzip -c -" | gunzip -c - | tar -xf -</pre>
With the same data as above, this method took smaller amount of time, 1:13 seconds to be exact.