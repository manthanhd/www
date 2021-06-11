---
id: 601
title: Setting up an OAuth2 provider
date: 2017-04-20T11:55:24+01:00
author: Manthan Dave
layout: post
guid: https://www.manthanhd.com/?p=601
permalink: /2017/04/20/setting-up-an-oauth2-provider/
image: /wp-content/uploads/2017/04/what-is-oauth-800x420.jpg
categories:
  - Educational
tags:
  - oauth
  - oauth2
  - provider
  - service
---
In this post, we're going to talk about installing and setting up your very own OAuth2 provider. If you have used Facebook or Twitter logins, you'd know that they have their own OAuth2 providers. In reality, those are more than just OAuth2 providers as they also have OpenID Connect on them, however, that will be a post for another day.
<h2>Why would I want an OAuth2 provider?</h2>
Well, there are many reasons why you'd want an OAuth2 provider.
<ol>
 	<li>Because its cool.</li>
 	<li>Because its hip.</li>
 	<li>Because, why not?</li>
</ol>
On a more serious note, if you have a bunch of applications running in your house, you can use your own OAuth2 provider to provide identity and custom authorisations to every app in a way that if one of those apps gets compromised, it won't take your whole house down. This lets you operate all of your apps in a standard way.

Also, who in your family doesn't want "Sign in via &lt;insert_family_name&gt;" button? :P

For this post, we're going to use Forgerock's OpenAM version 13.

<!--more-->
<h2>Install prerequisites</h2>
OpenAM runs on tomcat so you need to install that first. We're going to use OpenAM 13 for this demo along with Tomcat 8 running on Java 8. So the running order is:
<ol>
 	<li>Download <a href="http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html">Java 8 JDK</a></li>
 	<li>Download <a href="https://tomcat.apache.org/download-80.cgi">Tomcat 8</a></li>
 	<li>Download <a href="https://backstage.forgerock.com/downloads/OpenAM/OpenAM%20Enterprise/13.0.0/OpenAM%2013/war#list">OpenAM 13 WAR (web archive)</a></li>
</ol>
<h2>Initial Setup</h2>
Before you start tomcat, copy the OpenAM 13 WAR into the tomcat <span class="lang:default decode:true crayon-inline ">webapps</span> directory. This directory is typically under <span class="lang:default decode:true crayon-inline ">$TOMCAT_HOME/webapps</span> where <span class="lang:default decode:true crayon-inline">$TOMCAT_HOME</span> is the root folder of where tomcat is installed. If you are unsure, <span class="lang:default decode:true crayon-inline ">$TOMCAT_HOME</span> folder will also contain other directories like <span class="lang:default decode:true crayon-inline ">bin, conf, lib</span> etc. When you copy the war over, make sure you rename it something memorable (and without any spaces or special characters) like <span class="lang:default decode:true crayon-inline">openam.war</span>. Whatever you rename this war to will become the context root of your web server. So if you rename the war file to <span class="lang:default decode:true crayon-inline">openam.war</span>, the openam server will become accessible at <span class="lang:default decode:true crayon-inline">http://localhost:8080/openam</span>.

Start tomcat by running the <span class="lang:default decode:true crayon-inline ">startup.sh</span> shell script located in <span class="lang:default decode:true crayon-inline ">$TOMCAT_HOME/bin</span> directory. To ensure that the startup was successful, open <span class="lang:default decode:true crayon-inline ">catalina.out</span> log file located in <span class="lang:default decode:true crayon-inline ">$TOMCAT_HOME/logs</span> directory. The war file is relatively large and could take some time to deploy. Make sure the log ends with something like this:
<pre class="lang:default decode:true ">INFO [main] org.apache.catalina.startup.Catalina.start Server startup in 80585 ms</pre>
and has a line like:
<pre class="lang:default decode:true ">org.apache.catalina.startup.HostConfig.deployWAR Deployment of web application archive /var/tomcat/webapps/openam.war has finished in 80,158 ms</pre>
somewhere above the startup log message.
<h2>Manually configure OAuth2 provider</h2>
<h3>Setup OAuth2 Provider</h3>
This is the quick and dirty way of configuring OAuth2 provider if you are using a server that is going to be long lived, as in, not a docker image. Once you do this, make sure you back up the disk to ensure that the settings are preserved.

In your web browser, navigate to the following URL:
<pre class="lang:default decode:true ">http://localhost:8080/openam</pre>
Go through the default configuration. Setup the admin account password. Make sure you remember this as there is no way to recover it afterwards if you lose it. I usually have the admin account credentials as follows:
<pre class="lang:default decode:true ">Username: amadmin
Password: password123
// Password simple for readability purposes</pre>
Once you have completed the initial setup, it will take you to the login screen. Login with your admin credentials and you will be taken to the OpenAM console. Here, click on the "Top Level Realm /" to configure the realm.

At this point, you should be able to see this:

[caption id="attachment_602" align="aligncenter" width="700"]<a href="https://www.manthanhd.com/wp-content/uploads/2017/04/forgerock-console-home.png"><img class="size-large wp-image-602" src="https://www.manthanhd.com/wp-content/uploads/2017/04/forgerock-console-home-700x473.png" alt="" width="700" height="473" /></a> OpenAM Realm Overview[/caption]

In the common tasks pane, click "Configure OAuth Provider" button. From there, click "Configure OAuth 2.0" button. You should see something like this:

[caption id="attachment_603" align="aligncenter" width="700"]<a href="https://www.manthanhd.com/wp-content/uploads/2017/04/forgerock-configure-oauth-provider.png"><img class="size-large wp-image-603" src="https://www.manthanhd.com/wp-content/uploads/2017/04/forgerock-configure-oauth-provider-700x472.png" alt="" width="700" height="472" /></a> Configure OAuth Provider[/caption]

On this screen, you can configure your OAuth2 provider however you want. For simplicity reasons, lets just keep everything as default and click "Create" button. For me, this action takes less than a second. Click "OK" on the pop up modal.

Now you should be back to the OpenAM console realm overview page.
<h3>Setup OAuth Clients</h3>
If you have used any public OAuth2 providers like Facebook and Twitter, you should be familiar with OAuth clients. A client is an application that wants to access user data from the OAuth provider. Each client must first authenticate itself using a set of credentials and then request authorisation using scopes to access a set of resources.

The first part, authentication, is achieved by issuing a set of credentials which are, usually, a clientId and clientSecret. We need to create a client and then issue a clientId and clientSecret for it to use.

From the OpenAM dashboard, click on "Agents" on the left. In the resulting page, click on "OAuth 2.0/OpenID Connect Client" tab on the second tab row from the top.

[caption id="attachment_604" align="aligncenter" width="700"]<a href="https://www.manthanhd.com/wp-content/uploads/2017/04/forgerock-add-oauth2-client.png"><img class="wp-image-604 size-large" src="https://www.manthanhd.com/wp-content/uploads/2017/04/forgerock-add-oauth2-client-e1492091846601-700x336.png" alt="" width="700" height="336" /></a> Add OAuth2 Client[/caption]

Click "New" under the Agent section. Here, Name refers to the ClientId and Password refers to the ClientSecret. Once you've created a strong secret, click "Create". This will take you back to the "OAuth 2.0/OpenID Connect Client" agents screen. Click into the agent that you just created.

Now this is the screen where you can fully configure almost every aspect about your client. You must explicitly whitelist the redirectionUri for your client as well as the scopes that are allowed for the client to access.

Ideally you should repeat this for every client application you have such that each application has a unique set of clientId and clientSecret.
<h2>Add users</h2>
Currently, you won't have many users in your OAuth2 provider. Out of the box OpenAM will come with an embedded OpenDJ instance. At its core, OpenDJ is an LDAP data store with a REST interface around it.

The easiest way to add users is to click on "Subjects" item in the left pane on the OpenAM console realm overview page.

Under "Users" section, click on "New". This should open up a "New User" screen where you can add some details about your user. Also, in case you didn't notice, your OpenAM instance comes built in with a "demo" user. The "demo" user has its default password set to "changeit".

Now that you have added some users and have a client, you should be able to write an OAuth2 client app that allows you to "Sign In" via your OAuth2 provider instance!