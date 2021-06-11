---
id: 105
title: 'What to do when app downloads in Windows 8/8.1 store get stuck at &#8220;Pending&#8221; or &#8220;Downloading&#8221; stage?'
date: 2015-08-24T19:41:24+01:00
author: Manthan Dave
layout: revision
guid: http://www.manthanhd.com/2015/08/24/12-revision-v1/
permalink: /?p=105
---
So I had this problem today. Well to be honest, I had this problem before as well but I decided to fix it today. Earlier today, I upgraded to Windows 8.1 and I was hoping that the upgrade would resolve this problem. Well, it didn't. After surfing through the web, I found the solution. So, here it goes:

First of all, you need to be in safe mode. Open the charms bar, click on the power button, hold the <code>shift</code> key and while you're holding it, click <code>"Restart"</code>. This will restart Windows in troubleshooting/recovery mode.
<!--more-->
From the menu that has been presented to you, click <code>"Troubleshoot"</code>. Now select <code>"Advanced options"</code> and from the subsequent screen click <code>"Windows Startup settings"</code>. Finally, click <code>"Restart"</code> button.

Now, if you have Windows 8 installed, you will have an old command prompt styled menu from which you can select <code>"Safe Mode with Networking"</code> by pressing navigational arrow keys. However, if you have Windows 8.1, then you will have to press number 5 key on your keyboard to go into <code>"Safe Mode with Networking"</code>. Once you're there, fire up command prompt. Type in:
<code>cd %systemroot%</code>

Then type:
<code>ren SoftwareDistribution SoftwareDistribution.old</code>

That's it. Restart the computer by using the charms menu. This time, don't hold down the shift key.

When Windows normally boots up, open the store, wait a few seconds and try updating/downloading apps and it should work just fine.