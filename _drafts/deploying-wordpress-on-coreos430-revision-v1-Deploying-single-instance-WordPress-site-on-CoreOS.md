---
id: 459
title: Deploying single instance WordPress site on CoreOS
date: 2016-09-14T21:53:05+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2016/09/14/430-revision-v1/
permalink: /?p=459
---
<p style="text-align: justify;">Lets start off by deploying a single MySQL DB instance. For this, we're going to use the default mysql image from docker hub. In order to deploy anything to CoreOS, you need to first create a service file. Here's one for the MySQL that we're going to deploy.</p>

<pre class="lang:default decode:true" title="wpdb.service">[Unit]
Description=MySQL database for wordpress

[Service]
ExecStartPre=-/usr/bin/docker stop wpdb
ExecStartPre=-/usr/bin/docker rm wpdb
ExecStartPre=-/usr/bin/docker pull mysql:latest
ExecStart=/usr/bin/docker run -e MYSQL_ROOT_PASSWORD=glory86 --name wpdb -t mysql:latest


[X-Fleet]
Conflicts=wpdb.service</pre>
<p style="text-align: justify;">Save that file as <span class="lang:default decode:true crayon-inline">wpdb.service</span>. Lets examine that file. As you can see, the file is split into three distinct sections. <span class="lang:default decode:true crayon-inline">Unit</span>, <span class="lang:default decode:true crayon-inline">Service</span> and <span class="lang:default decode:true crayon-inline">X-Fleet</span>. The Unit section tells CoreOS, or more specifically <span class="lang:default decode:true crayon-inline">fleet</span>, what this service is about and what it is for. Since this one is quite simple, we only have a <span class="lang:default decode:true crayon-inline ">Description</span> here.<!--more--></p>
<p style="text-align: justify;">The second section is Service. This basically defines the mysql service. We've got four lines. The first three cover <span class="lang:default decode:true crayon-inline">ExecStartPre</span> section where we define commands that it needs to run before starting our service. In this case, we are stopping and removing any existing containers named <span class="lang:default decode:true crayon-inline ">wpdb</span> (as that's what we're calling our database instance). We're also pulling in the latest mysql image from dockerhub so that we can be prepared when the service is started.</p>
<p style="text-align: justify;">Next we're defining what happens when the service is started by defining the <span class="lang:default decode:true crayon-inline">ExecStart</span> property. Here, we're running a simple docker run command. Since the mysql image needs a password to be defined in the environment, we're passing one here. We're also naming our database to be <span class="lang:default decode:true crayon-inline">wpdb</span>. Note that we are not binding any ports as we don't want access to the mysql locally on any machine. Later on, we'll let our wordpress docker container talk directly to the mysql container by using docker <span class="lang:default decode:true crayon-inline">link</span>.</p>
<p style="text-align: justify;">Now load up the <span class="lang:default decode:true crayon-inline">wpdb.service</span> using <span class="lang:default decode:true crayon-inline ">fleetctl</span> command:</p>

<pre class="lang:default decode:true">fleetctl load wpdb.service</pre>
<p style="text-align: justify;">Once loaded, you can start it by running the <span class="lang:default decode:true crayon-inline ">start</span> command:</p>

<pre class="lang:default decode:true ">fleetctl start wpdb.service</pre>
<p style="text-align: justify;">Now that our database container is deployed, lets deploy our wordpress container. To do that, we need another service file.</p>

<pre class="lang:default decode:true" title="www-manthanhd.service">[Unit]
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
<p style="text-align: justify;">Save that file as <span class="lang:default decode:true crayon-inline">www-manthanhd.service</span>. As you can see, this file is very similar to the <span class="lang:default decode:true crayon-inline ">wpdb.service</span> file that we created earlier. However, there are a few changes.</p>
<p style="text-align: justify;">Since we want to make sure that our database container is running before we run our wordpress container, we have to make sure that we have <span class="lang:default decode:true crayon-inline ">After</span> and <span class="lang:default decode:true crayon-inline">Requires</span> properties defined accordingly. In this case, we're instructing fleet to run our wordpress container that we require the <span class="lang:default decode:true crayon-inline ">wpdb.service</span> and to run this container after the <span class="lang:default decode:true crayon-inline">wpdb.service</span>.</p>
<p style="text-align: justify;">Another notable change is in the <span class="lang:default decode:true crayon-inline ">X-Fleet</span> section. Here we're defining an extra property called <span class="lang:default decode:true crayon-inline ">MachineOf</span> where we're telling fleet to deploy this wordpress container on the same machine as the <span class="lang:default decode:true crayon-inline ">wpdb.service</span> container. This is to ensure minimum latency between wordpress and the database.</p>
<p style="text-align: justify;">We do have a slight change in the <span class="lang:default decode:true crayon-inline ">ExecStart</span> where we're now linking the <span class="lang:default decode:true crayon-inline">wpdb</span> container to the wordpress container by using docker link as well as binding port <span class="lang:default decode:true crayon-inline">80</span>.</p>
<p style="text-align: justify;">Once done, load up the wordpress container service using <span class="lang:default decode:true crayon-inline">fleetctl</span>:</p>

<pre class="lang:default decode:true ">fleetctl load www-manthanhd.service</pre>
<p style="text-align: justify;">And then start it:</p>

<pre class="lang:default decode:true ">fleetctl start www-manthanhd.service</pre>
<p style="text-align: justify;">Lets check if the database and the wordpress containers are actually running. To do that, run:</p>

<pre class="lang:default decode:true ">fleetctl list-units</pre>
<p style="text-align: justify;">You should see output similar to this:</p>

<pre class="lang:default decode:true " title="fleetctl list-units">UNIT MACHINE ACTIVE SUB
wpdb.service 7528fd75.../172.17.8.101 active running
www-manthanhd.service 7528fd75.../172.17.8.101 active running</pre>
<p style="text-align: justify;">That's telling us that <span class="lang:default decode:true crayon-inline ">wpdb.service</span> and <span class="lang:default decode:true crayon-inline">www-manthanhd.service</span> are both running on the same machine - <span class="lang:default decode:true crayon-inline ">172.17.8.101</span> and are actually running!</p>
<p style="text-align: justify;">Cool! Lets try to see if we can access wordpress. You can do this by navigating to port <span class="lang:default decode:true crayon-inline ">80</span> of <span class="lang:default decode:true crayon-inline">172.17.8.101</span> or whatever ip address is shown above for <span class="lang:default decode:true crayon-inline ">www-manthanhd.service</span> above for you.</p>