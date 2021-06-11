---
id: 426
title: Some quick tomcat shell functions
date: 2016-08-27T18:12:02+01:00
author: Manthan Dave
layout: post
guid: https://www.manthanhd.com/?p=426
permalink: /2016/08/27/some-quick-tomcat-shell-functions/
categories:
  - Findings
tags:
  - bash
  - linux
  - shell
  - unix
---
Here are some bash profile functions to help you use tomcat as a pro. The following needs to be done in either your <span class="lang:default decode:true crayon-inline ">.profile</span> file or <span class="lang:default decode:true crayon-inline">.bash_profile</span>, depending on what shell you have and whichever method you prefer. First off, make sure you have exported <span class="lang:default decode:true crayon-inline ">CATALINA_HOME</span> variable pointing to the base of your tomcat installation.
<pre class="lang:sh decode:true">export CATALINA_HOME=/opt/tomcat</pre>
Once you've got that done, you can define the following shell functions.<!--more-->
<pre class="lang:sh decode:true">function tomst() {
  ${CATALINA_HOME}/bin/startup.sh
}

function tomsh() {
    ${CATALINA_HOME}/bin/shutdown.sh
}

function tomlog() {
    less ${CATALINA_HOME}/logs/catalina.out
}

function tomls() {
    ls -l ${CATALINA_HOME}/webapps | grep -v .war$
}

function tomlogf() {
    tail -f ${CATALINA_HOME}/logs/catalina.out
}

function tomps() {
    ps -ef | grep tomcat | grep -v grep | cut -d' ' -f2
}

function tomkill() {
    kill `tomps`
}</pre>
Lets break these down. The first command, <span class="lang:default decode:true crayon-inline ">tomst</span> , will start up your tomcat using the <span class="lang:default decode:true crayon-inline ">startup.sh</span> script. Depending on your setup, you might prefer to use the startup.sh script over the service command. This is a neat way to do it from any directory.

Next is the <span class="lang:default decode:true crayon-inline ">tomsh</span> command which will help you shut down your tomcat container. Again, like the previous command, this is using the <span class="lang:default decode:true crayon-inline">shutdown.sh</span> script instead of the service command. You could replace both of these with the service commands if you prefer.

The <span class="lang:default decode:true crayon-inline ">tomlog</span> and <span class="lang:default decode:true crayon-inline">tomlogf</span> commands both display the <span class="lang:default decode:true crayon-inline ">catalina.log</span> log file, however the <span class="lang:default decode:true crayon-inline ">tomlogf</span> command will follow (<span class="lang:default decode:true crayon-inline">tail</span>) the log.

The <span class="lang:default decode:true crayon-inline ">tomls</span> command will show whats in your webapps directory. Now, this command will only show you your war files so these might not necessarily be deployed, but its a good way to list your deployments.

The <span class="lang:default decode:true crayon-inline ">tomps</span> command will show you the process ID of the running tomcat container. As of this moment, this only looks for the keyword tomcat in your processes. If you have multiple tomcat containers running, you will have to make the <span class="lang:default decode:true crayon-inline ">grep</span> more specific.

Lastly, the <span class="lang:default decode:true crayon-inline ">tomkill</span> command effectively runs the <span class="lang:default decode:true crayon-inline ">tomps</span> command but with a prefix of a kill command, effectively executing a kill on your tomcat process.