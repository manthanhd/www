---
id: 433
title: Deploying WordPress.com on CoreOS
date: 2016-09-08T20:22:41+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2016/09/08/430-revision-v1/
permalink: /?p=433
---
wpdb.service
<pre class="lang:default decode:true" title="wpdb.service">[Unit]
Description=MySQL database for wordpress

[Service]
ExecStartPre=-/usr/bin/docker stop wpdb
ExecStartPre=-/usr/bin/docker rm wpdb
ExecStartPre=-/usr/bin/docker pull mysql:latest
ExecStart=/usr/bin/docker run -e MYSQL_ROOT_PASSWORD=glory86 --name wpdb -t mysql:latest


[X-Fleet]
Conflicts=wpdb.service</pre>
www-manthanhd.service
<pre class="lang:default decode:true " title="www-manthanhd.service">[Unit]
Description=Wordpress blog server for www.manthanhd.com
After=wpdb.service
Requires=wpdb.service

[Service]
ExecStartPre=-/usr/bin/docker stop www-manthanhd-com
ExecStartPre=-/usr/bin/docker rm www-manthanhd-com
ExecStartPre=-/usr/bin/docker pull wordpress:latest
ExecStart=/usr/bin/docker run --name www-manthanhd-com --link wpdb:mysql -p 80:80 -e WORDPRESS_DB_USER=root -e WORDPRESS_DB_PASSWORD=glory86 -e WORDPRESS_DB_HOST=mysql -t wordpress:latest

[X-Fleet]
Conflicts=www-manthanhd.service
MachineOf=wpdb.service</pre>
&nbsp;

&nbsp;