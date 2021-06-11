---
id: 622
title: 'Download and store a website&#8217;s public cert into a Java keystore'
date: 2017-05-05T15:20:30+01:00
author: Manthan Dave
layout: post
guid: https://www.manthanhd.com/?p=622
permalink: /2017/05/05/download-and-store-a-websites-public-cert-into-a-java-keystore/
image: /wp-content/uploads/2017/05/ssl-banner-e1448642838695.png
categories:
  - Findings
tags:
  - linux
  - security
  - shell
  - ssl
  - unix
---
The other day I had to download a public cert from a web service's host and store it in my java keystore so that it can be trusted. Here's what I did:
<pre class="lang:sh decode:true">openssl s_client -showcerts -connect www.manthanhd.com:443 &lt; /dev/null 2&gt;/dev/null| sed -n -e '/BEGIN\ CERTIFICATE/,/END\ CERTIFICATE/ p' &gt; /tmp/www-manthanhd-com.cert
sudo keytool -import -file /tmp/www-manthanhd-com.cert -alias wwwmanthanhdcom -keystore /opt/java/jre/lib/security/cacerts -storepass changeit</pre>
The first line downloads the public cert from www.manthanhd.com and stores it in <span class="lang:default decode:true crayon-inline">/tmp/www-manthanhd-com.cert</span>.

Next, we're using keytool to import that certificate into the Java cacert keystore. I am only using sudo here because Java is installed as root. If in your case its not, you can just use the keytool command without the sudo prefix.

Also, on my test box, the java keystore has the default java keystore password which is changeit. Make sure this matches whatever your keystore password is.

Last but not least, the alias that the cert is imported against is important because this is what you will have to use to later find it. In this case I'm just using the hostname without any punctuations. This way, I can easily find any cert I want for any host if I need it.

Thanks to Jamie Tanna (<a href="https://jvt.me/" target="_blank" rel="noopener noreferrer">jvt.me</a>) and Jack Gough (<a href="https://www.testingsyndicate.com/" target="_blank" rel="noopener noreferrer">testingsyndicate.com</a>) for their help on this.