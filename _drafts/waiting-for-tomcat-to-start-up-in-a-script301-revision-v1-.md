---
id: 302
date: 2016-01-15T13:40:43+00:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2016/01/15/301-revision-v1/
permalink: /?p=302
---
Function to wait for tomcat to get started.
<pre class="lang:sh decode:true ">function isTomcatUp {
       
    # Use FIFO pipeline to check catalina.out for server startup notification rather than
    # ping with an HTTP request. This was recommended by ForgeRock (Zoltan).
   
    FIFO=/tmp/notifytomcatfifo
    mkfifo "${FIFO}" || exit 1
    {
        # run tail in the background so that the shell can
        # kill tail when notified that grep has exited
        tail -f ${target.tomcat.dir}/logs/catalina.out &amp;
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
&nbsp;

&nbsp;