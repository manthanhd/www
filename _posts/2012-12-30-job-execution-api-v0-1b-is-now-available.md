---
id: 34
title: Job Execution API v0.1b is now available!
date: 2012-12-30T19:09:00+00:00
author: Manthan Dave
layout: post
guid: http://www.manthanhd.com/2012/12/30/job-execution-api-v0-1b-is-now-available/
permalink: /2012/12/30/job-execution-api-v0-1b-is-now-available/
blogger_blog:
  - codeninjutsu.blogspot.com
blogger_author:
  - ManthanHD
blogger_permalink:
  - /2012/12/job-execution-api-v01b-is-now-available.html
blogger_internal:
  - /feeds/2026358850785924011/posts/default/5133802601565458741
categories:
  - Skills
tags:
  - java
---
I have been working on this for a while and now I have finally released it. This is 0.1 beta so it may contain bugs. However, I am working on it constantly and am trying to improve the API.<br /><br />Job Execution API is all about job execution in Java. You have jobs which comprise of tasks. Each task is a class implementing the ITask interface. For now, the task is not Async so tasks are executed sequentially, in linear pattern. I intend to provide basic tasks which you can use to create standard jobs. You can later extend these tasks and make your own.<br /><br />Download the API here and let me know how it goes:<br /><a href="https://www.box.com/s/6cnn93gkrjmuf39sp12d">https://www.box.com/s/6cnn93gkrjmuf39sp12d</a>