---
id: 456
title: Introduction to Docker
date: 2016-09-15T13:45:42+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2016/09/14/441-autosave-v1/
permalink: /?p=456
---
<p style="text-align: justify;">Deploying applications is a complex task. You have to create some VMs, be it on DigitalOcean or AWS, download and install necessary prerequesites and then deploy your application. It would be easy if it were to end there, however, it doesn't.</p>

<p style="text-align: justify;">Following this you have application maintenance which includes deploying updated versions of your application in addition to other things like bug fixes. And this is where the real complexity starts. Your updated version might need another dependent application to be installed which in turn might need a version of some random tool to be upgraded. You did what was necessary but then you find out that the deployment of the updated application failed because you forgot to do that one null check in some corner of your application. So frantically you download the previous version of your application and try to restore it only to find that it doesn't work anymore since you upgraded that random tool to support your newer application.</p>

<p style="text-align: justify;">While all this is happening either your entire application is unavailable because your application is single instance or if it is indeed multi-instance split by a loadbalancer, all the other instances are being stressed out because one of the node is down.</p>

<p style="text-align: justify;">And now you are thinking. Well, there has to be a better way.</p>

<p style="text-align: justify;">Well my friend, there is.</p>

<p style="text-align: justify;"><!--more--></p>

<p style="text-align: justify;"><em>*queue star wars intro soundtrack*</em></p>

<p style="text-align: justify;">Docker.</p>

<p style="text-align: justify;">Docker allows you to create and deploy application containers. What is an Application Container you ask? Well, you can think of an application container like a VM but unlike a fully fledged VM, its the bit that surrounds your application. So in essence, instead of it being a virtual machine, it is a virtual container for your application. This allows application containers to start up within seconds while a virtual machine could take minutes. Also, application containers take up much less RAM as they don't have to load entire operating system in memory but only the bits that surround your application.</p>

<p style="text-align: justify;">So how can these amazing and fast application containers help simplify your deployments? Here's how.</p>

<p style="text-align: justify;">Because creating and destroying application containers is a inexpensive process, both in terms of compute resources and time, it encourages a philosophy of disposable infrastructure. The idea is that instead of creating your infrastructure and taking care of it for its lifetime, you create it only when you need it and then destroy it when its not needed. Also, because all containers emerge from their respective Dockerfile(s), which is code, they can be versioned along side your actual application. This means that if you roll back to a previous version, you will deploy a docker container for that application using <em>that</em> version of the Dockerfile. Also, because the container contains your project and all of its dependencies, its self contained and can be stood up or torn down without any impact on any of the existing versions.</p>

<p style="text-align: justify;">We've been on about Dockerfile for a while now so before we go any further lets see what it actually looks like. Here's an example:</p>

<pre class="lang:default decode:true">FROM ubuntu:14.04

RUN apt-get update -y
RUN apt-get install -y openjdk-7-jre-headless

ADD build/libs/*.jar /app

EXPOSE 8080

ENTRYPOINT java -jar /app/*.jar</pre>

<p style="text-align: justify;">That is a simple Dockerfile that deploys a Spring Boot application in a ubuntu based container. Briefly, the above Dockerfile defines a container image. The first line defines that our image is <span class="lang:default decode:true crayon-inline ">FROM</span> (based on) ubuntu 14.04 base image. Upon creating the image, it <span class="lang:default decode:true crayon-inline">RUN</span>s <span class="lang:default decode:true crayon-inline ">apt-get update -y</span>  and <span class="lang:default decode:true crayon-inline">apt-get install -y openjdk-7-headless</span> commands. In addition to that, it adds (read copies) <span class="lang:default decode:true crayon-inline">*.jar</span> file from the present working directory to <span class="lang:default decode:true crayon-inline ">/app</span> folder to make it available in the container. Also, because we know that the application is going to run on port <span class="lang:default decode:true crayon-inline">8080</span>, we're <span class="lang:default decode:true crayon-inline">EXPOSE</span>(ing) (defining) that port so that it can be accessed from outside the container. Finally, we're defining the <span class="lang:default decode:true crayon-inline ">ENTRYPOINT</span> (command that docker will execute when the container is started) as a <span class="lang:default decode:true crayon-inline ">java -jar</span> command to execute our Spring Boot application.</p>

<p style="text-align: justify;">I'll go into the details about all the sections that comprise a Dockerfile some other time but for now, the thing to take away is that a Dockerfile forms basis of your container and is used to define what your application container image looks like. The image can then be used to create and run the container.</p>

<p style="text-align: justify;">I am using a custom spring boot application, however, if you follow the <a href="https://spring.io/guides/gs/rest-service/">REST tutorial </a>on Spring Boot Tutorials website, you should end up with the same project as me. Within your project, make sure that the Dockerfile is located at the root of the project (in same directory as the <span class="lang:default decode:true crayon-inline ">src</span> and <span class="lang:default decode:true crayon-inline">build</span> folders).</p>

<p style="text-align: justify;">So once you have a Dockerfile, you can run the following command to create your image. Make sure that your Dockerfile is called exactly as Dockerfile and you are executing the following command from the same folder as the Dockerfile.</p>

<pre class="lang:sh decode:true">docker build -t bootdemo2 .</pre>

<p style="text-align: justify;">The above command builds our image and tags it (defined by <span class="lang:default decode:true crayon-inline ">-t</span> option) with <span class="lang:default decode:true crayon-inline">bootdemo2</span> tag. When the version (tag) is unspecified, docker assumes <span class="lang:default decode:true crayon-inline">latest</span>. However, if you do want to specify one, you just need to replace <span class="lang:default decode:true crayon-inline">bootdemo2</span> with <span class="lang:default decode:true crayon-inline">bootdemo2:1.0</span> or rather, more generically, <span class="lang:default decode:true crayon-inline">bootdemo2:&lt;version&gt;</span>. So building the first version of the application would be like so:</p>

<pre class="lang:sh decode:true">docker build -t bootdemo2:1.0 .</pre>

<p style="text-align: justify;">And then subsequently, the next minor version would be like:</p>

<pre class="lang:sh decode:true ">docker build -t bootdemo2:1.2 .</pre>

<p style="text-align: justify;">In this case, just leave the tag blank to allow it to default to latest.</p>

<p style="text-align: justify;">If everything went OK, you'll be able to see your image listed within the list of docker images on your machine. This can be viewed using the following command:</p>

<pre class="lang:sh decode:true ">docker images</pre>

<p style="text-align: justify;">You should see something like this in your output:</p>

<pre class="lang:default decode:true">REPOSITORY              TAG                 IMAGE ID            CREATED             SIZE
bootdemo2               latest              4150e61520b2        27 hours ago        339.3 MB
</pre>

<p style="text-align: justify;">Amazing! You've created your first Docker image. Hurrah! Lets run it to start a container off of that image. Simply run:</p>

<pre class="lang:sh decode:true">docker run -p 8080:8080 --name myspringbootapp --rm -it bootdemo2</pre>

<p style="text-align: justify;">You should see output like following:</p>

<pre class="lang:default decode:true">  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v1.4.0.RELEASE)

2016-09-14 15:02:01.125  INFO 6 --- [           main] c.manthanhd.eventsmgmtapis.Application   : Starting Application on 4c6454648246 with PID 6 (/project/build/libs/events-0.1.0.jar started by root in /)
2016-09-14 15:02:01.129  INFO 6 --- [           main] c.manthanhd.eventsmgmtapis.Application   : No active profile set, falling back to default profiles: default
2016-09-14 15:02:01.265  INFO 6 --- [           main] ationConfigEmbeddedWebApplicationContext : Refreshing org.springframework.boot.context.embedded.AnnotationConfigEmbeddedWebApplicationContext@5b8ed9b6: startup date [Wed Sep 14 15:02:01 UTC 2016]; root of context hierarchy
2016-09-14 15:02:03.278  INFO 6 --- [           main] s.b.c.e.t.TomcatEmbeddedServletContainer : Tomcat initialized with port(s): 8080 (http)
2016-09-14 15:02:03.294  INFO 6 --- [           main] o.apache.catalina.core.StandardService   : Starting service Tomcat
2016-09-14 15:02:03.296  INFO 6 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet Engine: Apache Tomcat/8.5.4
2016-09-14 15:02:03.424  INFO 6 --- [ost-startStop-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2016-09-14 15:02:03.425  INFO 6 --- [ost-startStop-1] o.s.web.context.ContextLoader            : Root WebApplicationContext: initialization completed in 2163 ms
2016-09-14 15:02:03.591  INFO 6 --- [ost-startStop-1] o.s.b.w.servlet.ServletRegistrationBean  : Mapping servlet: 'dispatcherServlet' to [/]
2016-09-14 15:02:03.598  INFO 6 --- [ost-startStop-1] o.s.b.w.servlet.FilterRegistrationBean   : Mapping filter: 'characterEncodingFilter' to: [/*]
2016-09-14 15:02:03.598  INFO 6 --- [ost-startStop-1] o.s.b.w.servlet.FilterRegistrationBean   : Mapping filter: 'hiddenHttpMethodFilter' to: [/*]
2016-09-14 15:02:03.599  INFO 6 --- [ost-startStop-1] o.s.b.w.servlet.FilterRegistrationBean   : Mapping filter: 'httpPutFormContentFilter' to: [/*]
2016-09-14 15:02:03.599  INFO 6 --- [ost-startStop-1] o.s.b.w.servlet.FilterRegistrationBean   : Mapping filter: 'requestContextFilter' to: [/*]
2016-09-14 15:02:04.003  INFO 6 --- [           main] s.w.s.m.m.a.RequestMappingHandlerAdapter : Looking for @ControllerAdvice: org.springframework.boot.context.embedded.AnnotationConfigEmbeddedWebApplicationContext@5b8ed9b6: startup date [Wed Sep 14 15:02:01 UTC 2016]; root of context hierarchy
2016-09-14 15:02:04.087  INFO 6 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/events],methods=[GET]}" onto public org.springframework.http.ResponseEntity&lt;com.manthanhd.eventsmgmtapis.events.models.EventList&gt; com.manthanhd.eventsmgmtapis.events.controllers.EventsController.getEvents()
2016-09-14 15:02:04.088  INFO 6 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/events/{id}],methods=[GET]}" onto public org.springframework.http.ResponseEntity&lt;com.manthanhd.eventsmgmtapis.events.models.Event&gt; com.manthanhd.eventsmgmtapis.events.controllers.EventsController.getSingleEvent(java.lang.String)
2016-09-14 15:02:04.088  INFO 6 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/events],methods=[POST]}" onto public org.springframework.http.ResponseEntity&lt;com.manthanhd.eventsmgmtapis.events.models.Event&gt; com.manthanhd.eventsmgmtapis.events.controllers.EventsController.createNewEvent(com.manthanhd.eventsmgmtapis.events.models.Event)
2016-09-14 15:02:04.088  INFO 6 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/error],methods=[GET]}" onto public org.springframework.http.ResponseEntity&lt;java.lang.String&gt; com.manthanhd.eventsmgmtapis.events.controllers.EventsController.error()
2016-09-14 15:02:04.091  INFO 6 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/error],produces=[text/html]}" onto public org.springframework.web.servlet.ModelAndView org.springframework.boot.autoconfigure.web.BasicErrorController.errorHtml(javax.servlet.http.HttpServletRequest,javax.servlet.http.HttpServletResponse)
2016-09-14 15:02:04.091  INFO 6 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/error]}" onto public org.springframework.http.ResponseEntity&lt;java.util.Map&lt;java.lang.String, java.lang.Object&gt;&gt; org.springframework.boot.autoconfigure.web.BasicErrorController.error(javax.servlet.http.HttpServletRequest)
2016-09-14 15:02:04.126  INFO 6 --- [           main] o.s.w.s.handler.SimpleUrlHandlerMapping  : Mapped URL path [/webjars/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
2016-09-14 15:02:04.126  INFO 6 --- [           main] o.s.w.s.handler.SimpleUrlHandlerMapping  : Mapped URL path [/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
2016-09-14 15:02:04.171  INFO 6 --- [           main] o.s.w.s.handler.SimpleUrlHandlerMapping  : Mapped URL path [/**/favicon.ico] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
2016-09-14 15:02:04.396  INFO 6 --- [           main] o.s.j.e.a.AnnotationMBeanExporter        : Registering beans for JMX exposure on startup
2016-09-14 15:02:04.465  INFO 6 --- [           main] s.b.c.e.t.TomcatEmbeddedServletContainer : Tomcat started on port(s): 8080 (http)
2016-09-14 15:02:04.470  INFO 6 --- [           main] c.manthanhd.eventsmgmtapis.Application   : Started Application in 4.07 seconds (JVM running for 4.811)
</pre>

<p style="text-align: justify;">Hurrah! Your container is running. Before we open our champagne bottles, lets break down that command to better understand what we just did.</p>

<p style="text-align: justify;">The base command is docker run which tells it to run something. All the parameters that follow tells it what to run.</p>

<p style="text-align: justify;">First one is the <span class="lang:default decode:true crayon-inline ">-p</span> parameter. Remember that <span class="lang:default decode:true crayon-inline">EXPOSE 8080</span>? Well that was exposed on the container. However, if you want that port to be available on your host (similar to port forwarding), you'd need this bit. In the <span class="lang:default decode:true crayon-inline">8080:8080</span>, the first part (before the colon) is the port on the container and the second part (after the colon) is the port on your machine. This mapping is optional. If you don't provide this value, it won't bind the port to your local port so to access your container you'll have to specify your container IP address instead of just <span class="lang:default decode:true crayon-inline">localhost</span>.</p>

<p style="text-align: justify;">Next is the name of the running container. This is entirely optional. If you don't specify one, it will generate a random name.</p>

<p style="text-align: justify;">After that is the <span class="lang:default decode:true crayon-inline ">--rm</span> parameter. Again, this is optional as it tells Docker to remove the container after its stopped. By default, Docker will keep a container around so that you can inspect its logs to determine why it was shut down. For our test we just need it to run our container and then remove it when we don't need it.</p>

<p style="text-align: justify;">Last set of parameters, <span class="lang:default decode:true crayon-inline">-i</span> and <span class="lang:default decode:true crayon-inline">-t</span> (combined into <span class="lang:default decode:true crayon-inline">-it</span>) tells Docker to run our container in interactive mode (<span class="lang:default decode:true crayon-inline">-i</span>) with a psuedo TTY (<span class="lang:default decode:true crayon-inline">-t</span>). We've chosen interactive mode to make it easier for us to see the output and terminate the container as we can just press <span class="lang:default decode:true crayon-inline ">Ctrl + C</span> to stop it instead of running <span class="lang:default decode:true crayon-inline">docker stop &lt;containerId&gt;</span> command.</p>

<p style="text-align: justify;">Lastly, we specify the image we want to create a container from. In this case its <span class="lang:default decode:true crayon-inline ">bootdemo2</span>. Since we haven't specified a tag, docker assumes its latest. If we do want to specify one, say, 1.0, it would be like <span class="lang:default decode:true crayon-inline">bootdemo2:1.0</span>.</p>

<p style="text-align: justify;">To check everything that Docker is currently running, you can run the following in a new tab.</p>

<pre class="lang:sh decode:true">docker ps -a</pre>

<p style="text-align: justify;">You should get something like:</p>

<pre class="lang:default decode:true ">CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
be82e8618345        bootdemo2           "/bin/sh -c 'java -Dj"   33 seconds ago      Up 31 seconds       0.0.0.0:8080-&gt;8080/tcp   myspringbootapp</pre>

<p style="text-align: justify;">Also, just quickly, if you get the following error:</p>

<pre class="lang:default decode:true">Cannot connect to the Docker daemon. Is the docker daemon running on this host?</pre>

<p style="text-align: justify;">Just update your host environment with the following command:</p>

<pre class="lang:sh decode:true">eval $(docker-machine env default)</pre>

<p style="text-align: justify;">and then re-run the above <span class="lang:default decode:true crayon-inline ">docker ps -a</span> command.</p>

<p style="text-align: justify;">To check whether or not the application is working fine, you could just navigate to your greeting endpoint (in my case <span class="lang:default decode:true crayon-inline">http://localhost:8080/events</span>) in your browser. This should work if you've passed in the <span class="lang:default decode:true crayon-inline ">-p</span> parameter. However, if you are using <span class="lang:default decode:true crayon-inline ">boot2docker</span> or docker on a non-linux based operating system, you will have to make sure that your docker-machine has port forwarding setup for <span class="lang:default decode:true crayon-inline">8080</span> (or whatever port you are trying to setup).</p>

<p style="text-align: justify;">As you can see, we've got one docker container running. Because we are running it in interactive mode, there are two ways of stopping it. You can stop is by pressing <span class="lang:default decode:true crayon-inline">Ctrl + C</span> command in the window where its running interactively, but as you guessed it, this only works when its running in interactive mode. Normal way to stop your container is by using the docker stop command.</p>

<pre class="lang:sh decode:true">docker stop &lt;container name / ID&gt;</pre>

<p style="text-align: justify;">You can obtain the container name from whatever you named your container as, or if you didn't explicitly name it, you can obtain the ID from the <span class="lang:default decode:true crayon-inline">docker ps</span> command.</p>

<p style="text-align: justify;">Also, because we started it with <span class="lang:default decode:true crayon-inline ">--rm</span> command, doing docker stop will remove the container as well. However, if we didn't have that flag in, you'd have to run the below command after the stop command:</p>

<pre class="lang:sh decode:true ">docker stop &lt;container name / ID&gt;</pre>

<p style="text-align: justify;">Once you've stopped your container, you could try re-running it but this time without the <span class="lang:default decode:true crayon-inline">--rm</span> flag and see the difference when it comes to stopping your container.</p>

<p style="text-align: justify;">Relating back to our earlier analogy of problems with deploying different versions of a single application, using docker, since you would have a docker image for every version of your application, you'd just swap out an old container with a new one. Even better than that, you could try out the any version of your application locally and since its a container, it would run in exactly the same way in production. If things don't work out, just remove that version of the container and re-deploy a version that works.</p>

<p style="text-align: justify;">Simple! I hope this post has given you a good introduction to Docker and the problem it solves for you. I'd love to hear about your experiences with Docker. Drop a note down in the comments below or on my twitter account <a href="https://twitter.com/davemanthan">@davemanthan</a>.</p>