---
id: 556
title: Generating password from linux command line
date: 2017-01-23T16:31:55+00:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2017/01/23/546-revision-v1/
permalink: /?p=556
---
Managing production passwords isn't a trivial task. I was trying to deploy a containerized app the other day that had a database deployed with it. During the deployment, I was trying to find an easy way to set a secure password. I didn't want anyone to know the password because I wanted only the application to know it and no one else. Also, the container was setup in a way that the database cannot be accessed from the outside world.

So instead of hard-coding the password, after doing some research, I used the following command:<!--more-->
<pre class="lang:sh decode:true">&lt; /dev/urandom tr -dc _A-Z-a-z-0-9+= | head -c${1:-32};echo;</pre>
This might look like some gibberish but it uses linux's <span class="lang:default decode:true crayon-inline ">/dev/urandom</span> to generate some randomness and extracts human readable characters like alphabets, numbers and common symbols like underscores, hyphens, pluses and equals.

Try it! it works!

Now, using clever bash interpolation syntax you can embed this password throughout your script in a secure way.