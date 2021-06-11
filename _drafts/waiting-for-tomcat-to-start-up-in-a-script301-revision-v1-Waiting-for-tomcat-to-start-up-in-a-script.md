---
id: 304
title: Waiting for tomcat to start up in a script
date: 2016-01-15T13:49:38+00:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2016/01/15/301-revision-v1/
permalink: /?p=304
---
So here's something that I have found that waits for tomcat to come up but rather than polling or a static time based wait, it uses FIFO pipeline to wait.
<pre class="lang:sh decode:true">function isTomcatUp {
       
    # Use FIFO pipeline to check catalina.out for server startup notification rather than
    # ping with an HTTP request. This was recommended by ForgeRock (Zoltan).
   
    FIFO=/tmp/notifytomcatfifo
    mkfifo "${FIFO}" || exit 1
    {
        # run tail in the background so that the shell can
        # kill tail when notified that grep has exited
        tail -f $CATALINA_HOME/logs/catalina.out &amp;
        # remember tail's PID
        TAILPID=$!
        # wait for notification that grep has exited
        read foo &lt;${FIFO}
        # grep has exited, time to go
        kill "${TAILPID}"
    } | {
        grep -m 1 "INFO: Server startup"
        # notify the first pipeline stage that grep is done
        echo &gt;${FIFO}
    }
    # clean up
    rm "${FIFO}"
}</pre>
Drop this into a tomcat-util.sh file, make it executable and then source the file:
<pre class="lang:sh decode:true ">chmod u+x tomcat-util.sh
source tomcat-util.sh</pre>
You'll now have isTomcatUp available as a bash command.

&nbsp;