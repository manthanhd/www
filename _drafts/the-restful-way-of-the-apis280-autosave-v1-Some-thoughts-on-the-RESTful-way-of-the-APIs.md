---
id: 297
title: Some thoughts on the RESTful way of the APIs
date: 2016-01-11T07:28:34+00:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2016/01/11/280-autosave-v1/
permalink: /?p=297
---
<p style="text-align: justify;">So, it's been a while now since I have written a post reflecting my thoughts on a subject. It has been primarily like this since I've been busy doing what I always do - trying out new cool things and ideas.</p>
<p style="text-align: justify;">In almost every app, every SaaS platform or every web based backend system I write, the first thing I start with is an API. This API first attitude has helped me in many ways. First and foremost, it helps me think how the architecture must work to suffice the needs of the client. It also leads me to think about containers and how best to distribute load etc. Now, this might seem like an overkill when just trying out a new idea but it at least gives me a rough estimate of the scope of the project that I'm looking at. Besides, now a days, working with thick client frameworks like Angular JS almost demands that you have a strong backend with a well written collection of APIs.</p>
<p style="text-align: justify;">Writing restful APIs time and time again have helped me improve my understanding regarding the idea of "restful APIs" with every iteration and hence, I've come up with few thoughts regarding ways in which a RESTful API should be written. So, here we go.</p>
<p style="text-align: justify;"><strong>A URL path should contain singular nouns and nothing else. </strong>This comes straight out of the REST specification. However, there have been cases where I have been forced to use verbs but that has only been the case when I haven't been very clear about what that endpoint is doing. Before defining your endpoints, be very clear on what you are trying to achieve. For instance, once I was writing an endpoint for an application to process an incoming web hook. Hence, I named the endpoint "POST: /process". While I actually did implement this, it kept bugging me throughout the implementation of the application. Finally, when that piece of functionality was coming to an end did I get crystal clear understanding regarding what that endpoint was doing. When a web hook gets triggered, things happen within the application. THAT thing is an Event! Without doubt, I quickly renamed the endpoint to "POST: /event" and my mind was at peace. In retrospect, I think I should've created a separate endpoint for each application consuming that endpoint.</p>
<p style="text-align: justify;">While developing one of the APIs, I had a devious idea. Normally when defining an endpoint that allows one to retrieve a resource, say for instance a post object, one's target endpoint might look like "GET: /post/1" where "post" is the resource and "1" is the ID of the resource. But what if I wanted to select posts by their category ID? Would my target endpoint look like "GET: /post/1"? If yes then whats the difference between the first and second call? How about "GET: /post/byCategory/1"? Or even "GET: /postsByCategory/1"?</p>
<p style="text-align: justify;">For some time I had a crazy obsession with such endpoints to an extent that quite a few endpoints during that time were defined like this. However, after a while these endpoints just started to look weird. Also, when I started developing the frontend AngularJS application, making queries started to feel more difficult than it should've been. For instance, I had a dropdown that allowed a user to select the search type (category|tags|name) and then whatever the user typed in the search box got searched. Initially I had a horrible if|elseif|else blocks which chose which endpoint to call based on selected search type. However, this quickly became really boring, tedious and frankly, dirty. So, I revisited my endpoints again. If I, as someone who had made these endpoints struggled to develop an application using them, then what should I expect from the developers who will build their applications on my platform?</p>
<p style="text-align: justify;">Finally, I ended up having a couple of thoughts regarding how this should be done. "GET: /post?categoryId=5" was my final answer. This not only solved the problem of querying by multiple search parameters but also made it easier for the consuming application to call the endpoint. Now, this new endpoint meant that I only had to assign field names to each dropdown and the query would build itself!</p>
<p style="text-align: justify;"><b>Query strings represent things that you want to do to your resource. </b>This is normally fairly obvious and I will cover more of this in depth in later paragraphs but for starters, query strings can be nouns, adjectives or even verbs (rare) with values or switches on them. For instance, "GET: /person/1?gender=male" clearly means that give me a person with ID 1 whose gender is male.</p>
<p style="text-align: justify;">You can also do other neat things with query strings like provide metadata regarding your call. For instance, "GET: /person/1?_select=firstname,lastname" means give me person whose ID is 1 and only select firstname and lastname fields of each object. This kind of functionality is especially useful as it helps clients save their precious processing power, memory and network bandwidth. Think of small micro controllers displaying temperature information or mobile apps running on 2G or even Edge network. Any query parameter indicating metadata should begin with an underscore so that they can easily be differentiated in a pool of query parameters.</p>
<p style="text-align: justify;">Never ever build a query within a query. I saw some examples on the web where someone had built an endpoint like "GET: /post/?query=startsWith:Senate,hasMinWords:100". I mean, why? How does one even go thinking like this? This is a horrible idea because you're forcing your users to learn your query language even when the principles you're building this on (REST) provides a framework like query string for you to use. The above query within query can be broken down as "GET: /post?startsWith=Senate&amp;hasMinWords=100". This is perfectly acceptable.</p>
<p style="text-align: justify;">You could ask, what if I wanted to search posts by their post header, body and tag components? Well, you could just do "GET: /post?headerStartsWith=Senate&amp;bodyHasMinWords=100" or even "GET: /post?header.startsWith=Senate&amp;body.hasMinWords=100" although the first one reads better but might be slightly tricky to implement.</p>
<p style="text-align: justify;"><strong>Respond properly, don't be lazy. </strong>When it comes to responses, it's easy to forget how important it is to respond properly. In most of the APIs that I write, for every POST request, I like to respond with HTTP 200 returning the object instance that was created as a result of that request. The same goes for PUT, but not for DELETE. The name of that HTTP method, DELETE clearly means that the resource was deleted, i.e. it doesn't exist. It makes me sad when I see implementations online where people return the resource that was deleted. If it was deleted what can the client possibly use it for? Hence, for DELETE, the response should be HTTP 204 no content.</p>
<p style="text-align: justify;">Now, depending on how secure you want to make your API, the behaviour of your endpoints on error will differ. It is incredibly useful to respond with a stack trace in a development environment but probably not when it comes to production. It pays in the long run to respond in a meaningful way when something goes wrong. At the very least, you must respond with HTTP 400 when it is the client's fault, for example validation of the input data gone wrong. However, there are tons of other error codes you could use for other situations.</p>
<p style="text-align: justify;">Sometimes you might have a protected resource, something that requires a login. In situations such as these, there are two ways you can respond, 401 (Unauthorized) and 403 (Forbidden). Typically, I like to respond with 403 when user is not logged in, i.e. not authenticated and 401 when the user is authenticated but doesn't have enough privileges i.e. is not authorized to do that operation. Never ever redirect to login page or API endpoint when the client is trying to do something that requires login.</p>
<p style="text-align: justify;">There is no shame in responding with HTTP 500. Yes you read that right. HTTP 500 does not mean that your app has crashed. It only means that something has gone wrong at the server side and the server doesn't want to tell you about it. Think about it. If the app had crashed, you wouldn't receive any response. It'd just die. When writing any API during my first iteration, I just respond with 500s in all places where I think things might go wrong. I normally then go back and improve things one by one, handling errors carefully and responding in a helpful way.</p>
<p style="text-align: justify;">Now, a word on caching. As far as I know, most containers and frameworks handle caching internally. However, sometimes you might need to configure stuff at endpoint level. For instance, if your endpoint is outputting information that gets updated as part of a batch process that occurs infrequently, it will be incredibly useful to use the expires headers. Also, when responding to a request, if the if-modified-since header is indicating that the data that you have on the server hasn't changed since last time, respond with 304. This is incredibly useful especially when your client is a webapp running in a web browser as it will help improve client experience dramatically.</p>
<p style="text-align: justify;"><b>Login requests must always be a POST request. </b>I get this a lot. If you ask this to me a couple of years ago, I'd probably say that the objective of Login is to "get" a session and hence it should be a GET request. I don't even know how I got that logic but one of my colleagues opened my eyes when they mentioned that the objective of Login is not to GET a login session but to CREATE one and hence it is a POST request. Same goes for registration. Your response at this stage depends. You could either respond with the user object which contains information about the logged in user or you can just return with 204. However, bear in mind that you'll have to strip out sensitive information like passwords and security question answers when responding with the user object to avoid security nightmare.</p>
<p style="text-align: justify;">However, for consumer facing applications, game has slightly changed with the introduction of social logins. In this case, you'll be making everyone's life easier if the initial request to login via social network was a GET request. As a general rule of thumb, I normally go with "GET: /auth/&lt;provider name&gt;" type of format when introducing social logins where in case of Facebook, this becomes "GET: /auth/facebook".</p>
<p style="text-align: justify;">So I hope this has given you something to think about. Whe</p>