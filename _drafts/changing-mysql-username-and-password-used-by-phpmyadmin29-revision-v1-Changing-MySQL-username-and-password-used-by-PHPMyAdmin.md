---
id: 174
title: Changing MySQL username and password used by PHPMyAdmin
date: 2015-08-25T23:08:47+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2015/08/25/29-revision-v1/
permalink: /?p=174
---
So, I had to do this the other day and I got a bit confused. After poking around a bit, I finally found it. If you have xampp installed, go to the install directory. In my case, this directory is C:xampp which is the default directory. Locate the phpmyadmin folder and open it. Locate the config.inc.php file within the phpmyadmin folder. Open up the file. You need to make change in the following two lines:

<code>$cfg['Servers'][$i]['user'] = '<b><u>root</u></b>'; </code>
<code>$cfg['Servers'][$i]['password'] = '<b><u>password</u></b>' </code>

The underlined parts are the ones that you've got to change. This file contains a lot of admin stuff so make sure you look around a bit and understand what it does.

If you don't know what you are doing, ensure you take a backup of the file before changing things.