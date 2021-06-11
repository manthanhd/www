---
id: 196
title: API for recognizing gestures in Android
date: 2015-08-25T23:28:40+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2015/08/25/35-revision-v1/
permalink: /?p=196
---
I have been busy lately. For most of the time, I have been working on gesture API for android. Android already has a built in gesture API but to actually use it, you have to apply mathematics which makes it a bit comlicated for a normal Android developer. AGAPI (Accelerometer Gesture API) adds a layer of abstraction over the existing gesture API to make it easier to recognize gestures. Using AGAPI is very simple. Implement the ShakeListener or any gesture listener class. Register your listener to ShakeManager or corresponding gesture manager instance. Write your code in OnShake or in corresponding event methods and BAM! Your gesture rich application is ready! At this stage of development, the Shake gesture is ready to use. You can get the API <a href="http://code.google.com/p/ag-api/downloads/list" target="_blank">here</a>.<br /><br />Note: This API is licensed under Apache License 2.0.