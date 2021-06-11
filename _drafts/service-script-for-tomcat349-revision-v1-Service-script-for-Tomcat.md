---
id: 423
title: Service script for Tomcat
date: 2016-08-27T13:23:28+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2016/08/27/349-revision-v1/
permalink: /?p=423
---
Quick script to allow starting and stopping tomcat from the service command.

Create a script called <span class="lang:default decode:true crayon-inline ">tomcat7</span>  in <span class="lang:default decode:true crayon-inline ">/etc/init.d</span>  like so:
<pre class="lang:sh decode:true ">vim /etc/init.d/tomcat7</pre>
Here's what goes into the script:<!--more-->
<pre class="lang:sh decode:true">#!/bin/bash
#
# chkconfig: - 85 15
# description: Jakarta Tomcat Java Servlets and JSP server
# processname: tomcat
# pidfile: /var/www/tomcat/tomcat.pid

# Source function library.
. /etc/init.d/functions

TOMCAT_USER=tomcat
CATALINA_HOME=/var/tomcat
RETVAL=$?

case "$1" in
start)
        echo "Starting Tomcat"
        exec su -l $TOMCAT_USER -c "$CATALINA_HOME/bin/catalina.sh start"
;;

stop)
        echo "Stopping Tomcat"
        exec su -l $TOMCAT_USER -c "$CATALINA_HOME/bin/catalina.sh stop -force"

        numproc=`ps -ef | grep "$CATALINA_HOME/bin/bootstrap.jar" | grep -v grep |awk -F' ' '{ print $2 }'`;
          if [ $numproc ]; then
          echo "###kill pid: $numproc"
            kill -9 $numproc
          fi
;;

debug)
        echo "Starting Tomcat in debug"
        exec su -l $TOMCAT_USER -c "$CATALINA_HOME/bin/catalina.sh jpda start"
;;

restart)
        $0 stop
        sleep 15
        $0 start
;;

*)
        echo $"Usage: $0 {start|stop|restart}"
        exit 1
;;
esac

exit $RETVAL</pre>
The commands start, stop, debug and restart can be run via service. Example:
<pre class="lang:sh decode:true">service tomcat7 start</pre>
Then apply appropriate permissions:
<pre class="lang:sh decode:true">chmod 755 /etc/init.d/tomcat7

chown root:root /etc/init.d/tomcat7</pre>
&nbsp;