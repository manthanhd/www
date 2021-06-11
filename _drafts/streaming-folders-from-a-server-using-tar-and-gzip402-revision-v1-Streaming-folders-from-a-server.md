---
id: 405
title: Streaming folders from a server
date: 2016-08-22T13:14:52+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2016/08/22/402-revision-v1/
permalink: /?p=405
---
ssh ec2-user@xx.xxx.xx.xxx "sudo tar -cO /folder1 /opt/folder2" | tar -xf -

Time 1:56

ssh -i ~/.ssh/lao345-sandbox.pem ec2-user@10.122.64.165 "sudo tar -cO /prod/msp /opt/beasys | gzip -c -" | gunzip -c - | tar -xf -

Time: 1:13