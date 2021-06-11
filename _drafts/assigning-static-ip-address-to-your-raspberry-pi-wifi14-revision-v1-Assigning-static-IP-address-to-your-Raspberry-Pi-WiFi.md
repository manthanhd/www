---
id: 107
title: Assigning static IP address to your Raspberry Pi (WiFi)
date: 2015-08-24T19:41:53+01:00
author: Manthan Dave
layout: revision
guid: http://www.manthanhd.com/2015/08/24/14-revision-v1/
permalink: /?p=107
---
When I first bought my Raspberry Pi, I had this problem. My router and TV are in different rooms and I don't have a ethernet cable. This restricts networking ability of my Pi which is quite annoying. I searched for articles on assigning static IP addresses to Raspberry Pi but most of them were talking about assigning it for eth0. I, on the other hand wanted wlan0 static IP.

Now, I could have just got a long ethernet cable and solve this whole problem but I was just annoyed and was constantly asking myself "Why isn't this working over wifi". So finally, after significant digging around, I found the solution. Here it goes:
<!--more-->
First of all, take a backup of /etc/network/interfaces file:
<code>pi@raspberrypi ~ $ sudo su -
root@raspberrypi:~# cp /etc/network/interfaces .
</code>
<code>
</code>Now, in order to make this work, we need some information first. We need to find out the new static ip address that you want, gateway, mask, network and broadcast address. Assuming that you have a working wifi connection on your Raspberry Pi, type:
<code>root@raspberrypi:~# ifconfig</code>

From the output, the bit that we are interested in is:
<code>wlan0     Link encap:Ethernet  HWaddr 00:00:00:00:00:00
<b>inet addr:192.168.1.99  Bcast:192.168.1.255  Mask:255.255.255.0</b>
UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
RX packets:19153 errors:0 dropped:24512 overruns:0 frame:0
TX packets:25553 errors:0 dropped:0 overruns:0 carrier:0
collisions:0 txqueuelen:1000
RX bytes:3724822 (3.5 MiB)  TX bytes:31437802 (29.9 MiB)</code>
<code>
</code>The highlighted line will give you information about your current IP, broadcast and mask address. Take a note of the last two. So, in my case:
<b>address</b>: (this is the IP address you wish to reserve as static eg. 192.168.1.69)
<b>mask</b>: 255.255.255.0
<b>broadcast</b>: 192.168.1.255

To find out information about the last two bits, type:
<code>root@raspberrypi:~# netstat -nr
Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
0.0.0.0         <b>192.168.1.1</b>     0.0.0.0         UG        0 0          0 wlan0
<b>192.168.1.0</b>     0.0.0.0         255.255.255.0   U         0 0          0 wlan0</code>
<code>
</code>In my case:
<b>gateway</b>: 192.168.1.1
<b>network (a.k.a destination)</b>: 192.168.1.0

Now that we have all the information that we need, edit the /etc/network/interfaces file using nano. Putting all the bits and pieces together, it should look like:
<div style="clear: both; text-align: center;"><a style="margin-left: 1em; margin-right: 1em;" href="http://www.manthanhd.com/wp-content/uploads/2013/09/scr011.png"><img src="http://www.manthanhd.com/wp-content/uploads/2013/09/scr011-300x189.png" alt="" width="320" height="201" border="0" /></a></div>
Press Ctrl + O to save.
Now, since I was doing this whole process over SSH, I had to write a script to restart wifi because if I didn't, as soon as I turn it off, it would also take away my SSH access rendering me powerless. You can do the following in either case. Create a new file called restartwifi.sh using nano in root home directory:
<code>root@raspberrypi:~# nano restartwifi.sh</code>
<div style="clear: both; text-align: center;"><a style="margin-left: 1em; margin-right: 1em;" href="http://www.manthanhd.com/wp-content/uploads/2013/09/scr021.png"><img src="http://www.manthanhd.com/wp-content/uploads/2013/09/scr021-300x189.png" alt="" width="320" height="201" border="0" /></a></div>
Now, I am aware that this script can be improved but this is what I had before and it has worked without any problems. Press Ctrl + O to save the script. Then press Ctrl + X to exit.

Make sure you edit permissions to make the script executable. Run:
<code>root@raspberrypi:~# chmod +x restartwifi.sh</code>
<div></div>
<div>If you're not root, you'll have to prefix above command with "sudo".</div>
<div></div>
<div>Now comes the moment of truth. Run the script to reset your wifi by typing following command:</div>
<div><code>root@raspberrypi:~# nohup ./restartwifi.sh &amp;</code></div>
<div></div>
<div>If you're in putty session, you'll immediately go offline. If you've done everything correctly, you should be able to ssh into your Pi, after couple of minutes, on your chosen IP address.</div>