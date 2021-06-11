---
id: 106
title: Changing keyboard layout in Ubuntu Server (Linux, How to)
date: 2015-08-24T19:41:40+01:00
author: Manthan Dave
layout: revision
guid: http://www.manthanhd.com/2015/08/24/13-revision-v1/
permalink: /?p=106
---
So I recently installed Ubuntu Server in a virtual machine and I had problems with my keyboard layout. I use UK layout while the default that comes with Ubuntu Server is US layout. As you can imagine, this caused problems and I had to go hunt for a solution. Finally, I found one and here it is:

You need to reconfigure keyboard configuration. Type the following:
<code>sudo dpkg-reconfigure keyboard-configuration</code>
<!--more-->
If that doesn't work, you might need to install console-data package. Install it by typing:
<code>sudo apt-get install console-data</code>

and then try first step again.

If everything was fine, you should see this:
<div style="clear: both; text-align: center;"><a style="margin-left: 1em; margin-right: 1em;" href="http://www.manthanhd.com/wp-content/uploads/2013/09/scr01.png"><img src="http://www.manthanhd.com/wp-content/uploads/2013/09/scr01-300x167.png" alt="" width="320" height="177" border="0" /></a></div>
<div style="clear: both; text-align: justify;">Using this you can select brand of your keyboard. I am running this virtual machine on my Acer laptop and as you can see it is listed there. You can use up/down/page up/page down keys to navigate through the list. When you have picked one, press enter to go to next screen.</div>
<div style="clear: both; text-align: justify;"></div>
<div style="clear: both; text-align: justify;">You should now see this:</div>
<div style="clear: both; text-align: center;"><a style="margin-left: 1em; margin-right: 1em;" href="http://www.manthanhd.com/wp-content/uploads/2013/09/scr02.png"><img src="http://www.manthanhd.com/wp-content/uploads/2013/09/scr02-300x167.png" alt="" width="320" height="178" border="0" /></a></div>
<div style="clear: both; text-align: justify;">This is the language selection screen. Navigate through the list, select your language and as you did in previous one, press enter to move on to next screen.</div>
<div style="clear: both; text-align: justify;"></div>
<div style="clear: both; text-align: justify;">This is the screen where you select your keyboard type:</div>
<div style="clear: both; text-align: center;"><a style="margin-left: 1em; margin-right: 1em;" href="http://www.manthanhd.com/wp-content/uploads/2013/09/scr03.png"><img src="http://www.manthanhd.com/wp-content/uploads/2013/09/scr03-300x167.png" alt="" width="320" height="177" border="0" /></a></div>
<div style="clear: both; text-align: justify;">Assuming you're using a qwerty keyboard for PC, you should just leave the first one selected and press enter. However, if you are running this on a mac or your keyboard is of different kind, you are free to choose whatever applies to you from the list and continue by pressing enter.</div>
The next screen allows you to map a key on your keyboard to alternate grammar key (Alt Gr).
<div style="clear: both; text-align: center;"><a style="margin-left: 1em; margin-right: 1em;" href="http://www.manthanhd.com/wp-content/uploads/2013/09/scr04.png"><img src="http://www.manthanhd.com/wp-content/uploads/2013/09/scr04-300x166.png" alt="" width="320" height="176" border="0" /></a></div>
I have never used this key in my life for alternate grammar, however, hoping that I may use it at some point, I would map it to right-alt key (which actually is Alt Gr key). As usual, select one from the list and press enter.

Next one is compose key. Same drill. Pick one and press enter:
<div style="clear: both; text-align: center;"><a style="margin-left: 1em; margin-right: 1em;" href="http://www.manthanhd.com/wp-content/uploads/2013/09/scr05.png"><img src="http://www.manthanhd.com/wp-content/uploads/2013/09/scr05-300x166.png" alt="" width="320" height="177" border="0" /></a></div>
Compose key, if I remember correctly, allows you to type ASCII for certain characters when you press and hold it. This key on windows is left alt key however, here you can choose whatever you want for ubuntu server.

When you press enter, the wizard will quit. So that was the keyboard configuration screen which allows you to configure your keyboard pretty neatly. Have fun!