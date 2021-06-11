---
id: 130
title: Configuring Java HttpClient to use proxy
date: 2015-08-24T22:40:18+01:00
author: Manthan Dave
layout: revision
guid: http://www.manthanhd.com/2015/08/24/5-revision-v1/
permalink: /?p=130
---
So, for various security reasons, at work, I have to go through proxy in order to access anything. I was doing some prototyping with sparkpost and none of my code worked as by default the code wasn't going through the business proxy.

Naturally, I thought that I'd set HTTP_PROXY and HTTPS_PROXY environment variables and then run my java code. I did and as it is with all things in Software Engineering industry, it didn't work.

After several hours of googling (or what felt like several hours) I finally found what was wrong with it. In Java, due to security reasons, all proxy variables are ignored unless they have been explicitly set in code. Here's how you set them:<!--more-->
<pre class="lang:java">String proxyHost = System.getenv("HTTP_PROXY_HOST");
String proxyPort = System.getenv("HTTP_PROXY_PORT");
String proxyUser = System.getenv("HTTP_PROXY_USER");
String proxyPassword = System.getenv("HTTP_PROXY_PWRD");

CredentialsProvider credsProvider = new BasicCredentialsProvider();
credsProvider.setCredentials(new AuthScope(proxyHost, Integer.parseInt(proxyPort)),
        new UsernamePasswordCredentials(proxyUser, proxyPassword));

HttpHost proxyHostObject = new HttpHost(proxyHost, Integer.parseInt(proxyPort));

HttpClient client = HttpClientBuilder.create().setProxy(proxyHostObject)
    .setProxyAuthenticationStrategy(new ProxyAuthenticationStrategy())
    .setDefaultCredentialsProvider(credsProvider)
    .build();

HttpGet getRequest = new HttpGet(URL);
getRequest.addHeader("Authorization", API_KEY);
getRequest.addHeader("Content-Type", "application/json");
try {
    System.out.println("Executing request...");
    HttpResponse response = client.execute(getRequest);
    System.out.println("Request successfully executed.");

    HttpEntity entity = response.getEntity();
    String responseString = EntityUtils.toString(entity);
    System.out.println(responseString);
} catch (IOException e) {
    e.printStackTrace();
}</pre>
The CredentialsProvider class allows one to save the proxy credentials while the HttpHost class allows storing the proxy host.

I personally am not a fan of this arrangement as the proxyHost and proxyPort is being duplicated twice in both classes. If you find a better arrangement, feel free to drop me a line.

Thanks!