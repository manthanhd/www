---
id: 10
title: Rejecting a pairing request from a bluetooth device permanently on your Mac
date: 2014-09-05T09:36:00+01:00
author: Manthan Dave
layout: post
permalink: /2014/09/05/rejecting-a-pairing-request-from-a-bluetooth-device-permanently-on-your-mac/
blogger_blog:
  - codeninjutsu.blogspot.com
blogger_author:
  - ManthanHD
blogger_permalink:
  - /2014/09/rejecting-pairing-request-from.html
blogger_internal:
  - /feeds/2026358850785924011/posts/default/8701343290396984294
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
  - apple
  - mac
  - troubleshooting
---
So I had this problem the other day where I kept on getting pairing requests from my friend's bluetooth keyboard. This was rather annoying as the pairing dialog kept on popping up every few minutes. So I googled around a bit and found the following solution which worked for me.

Turn your bluetooth off. This will disable your bluetooth mouse and keyboard and hence you will have to use the built-in ones.

The problem is that at some point the keyboard that is nagging you to connect would have connected to your laptop. Your laptop remembers this and hence accepts incoming pairing request prompting you to verify it.
<!--more-->
Our aim is to make the machine forget that it was ever connected to this device. In order to do this, you will need to edit a file called com.apple.Bluetooth.plist. This file is located in /Library/Preferences and ~/Library/Preferences folder. This file is in binary so in order to be able to edit it, you will have to convert it to xml first. So, open up terminal and type:
<code>sudo plutil -convert xml1 /Library/Preferences/com.apple.Bluetooth.plist</code>

Now you can go on to finder or whatever you have and edit this file. Remove ... followed by ... tags which relate to the keyboard/device that you are trying to ban. Simple way is to find the name of the device in the file. This name will be in ... tags. Once you find the name, remove the whole container ... and preceding ... tags. Once you do this, save the file and convert it back to binary using the following terminal command:
<code>sudo plutil -convert binary1 /Library/Preferences/com.apple.Bluetooth.plist</code>

Do the same for the com.apple.Bluetooth.plist file located in ~/Library/Preferences folder as well. If that folder does not have this file, copy it over from /Library/Preferences folder.

Once you are done, turn the bluetooth on and now it shouldn't prompt you for pairing requests.