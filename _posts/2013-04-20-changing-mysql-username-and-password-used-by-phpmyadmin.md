---
id: 29
title: Changing MySQL username and password used by PHPMyAdmin
date: 2013-04-20T10:09:00+01:00
author: Manthan Dave
layout: post
guid: http://www.manthanhd.com/2013/04/20/changing-mysql-username-and-password-used-by-phpmyadmin/
permalink: /2013/04/20/changing-mysql-username-and-password-used-by-phpmyadmin/
blogger_blog:
  - codeninjutsu.blogspot.com
blogger_author:
  - ManthanHD
blogger_permalink:
  - /2013/04/changing-mysql-username-and-password.html
blogger_internal:
  - /feeds/2026358850785924011/posts/default/7691270208950874043
categories:
  - Findings
tags:
  - mysql
  - php
  - phpmyadmin
---
So, I had to do this the other day and I got a bit confused. After poking around a bit, I finally found it. If you have xampp installed, go to the install directory. In my case, this directory is C:xampp which is the default directory. Locate the phpmyadmin folder and open it. Locate the config.inc.php file within the phpmyadmin folder. Open up the file. You need to make change in the following two lines:

<code>$cfg['Servers'][$i]['user'] = '<b><u>root</u></b>'; </code>
<code>$cfg['Servers'][$i]['password'] = '<b><u>password</u></b>' </code>

The underlined parts are the ones that you've got to change. This file contains a lot of admin stuff so make sure you look around a bit and understand what it does.

If you don't know what you are doing, ensure you take a backup of the file before changing things.