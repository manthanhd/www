---
id: 386
title: 'Taking out Netflix&#8217;s gradle-lint-plugin for a spin'
date: 2016-06-26T15:46:15+01:00
author: Manthan Dave
layout: post
guid: https://www.manthanhd.com/?p=386
permalink: /2016/06/26/taking-out-netflixs-gradle-lint-plugin-for-a-spin/
categories:
  - Educational
  - Findings
tags:
  - ci
  - code efficiency
  - gradle
  - java
---
<p style="text-align: justify;">Dependency management is a complex thing. A typical gradle project has several dependencies, each of which in turn has multiple other dependencies and so on. This continues and what forms as a result is called a dependency graph. While this is great, unused dependencies is a big problem. For instance, say your project depends on dependency A which then depends on B, C and D. Your project also depends directly with another dependency E which in turn depends on F and G. The dependency graph would look something like this:</p>
<p style="text-align: justify;">Your Project -&gt; A -&gt; (B, C, D)
Your Project -&gt; E -&gt; (F, G)</p>
<p style="text-align: justify;">Now, your project probably only uses code in A which only then directly depends on some code in B. Also, maybe you also have some code that depends on E but that code in E doesn't depend directly on neither F, nor G. This means that C, D, F and G are your unused dependencies.<!--more--></p>
<p style="text-align: justify;">If you knew this from the start, you could just exclude those libraries. However, tracking this manually is incredibly difficult as the more complex the libraries are, the more complex the web of inter-dependencies becomes. I've been looking for an automated solution to this for a while and recently, I found that Netflix has a plugin that could help. This is called the gradle-lint-plugin.</p>
<p style="text-align: justify;">Its part of Netflix's OSS and is available on <a href="https://github.com/nebula-plugins/gradle-lint-plugin" target="_blank">github</a> to download. Its artifacts are available in <a href="https://bintray.com/bintray/jcenter" target="_blank">JCenter</a> maven repository.</p>
<p style="text-align: justify;"><a href="https://bintray.com/nebula/gradle-plugins/gradle-lint-plugin/_latestVersion"> <img src="https://api.bintray.com/packages/nebula/gradle-plugins/gradle-lint-plugin/images/download.svg" alt="" /></a></p>
<p style="text-align: justify;">To test it out, I quickly got a gradle project up and running by following the <a href="https://spring.io/guides/gs/spring-boot/" target="_blank">spring boot tutorial</a>. To make things a slightly more interesting, I added the <span class="lang:default decode:true crayon-inline ">org.apache:httpcomponents:httpclient</span> library as a dependency in my <span class="lang:default decode:true crayon-inline ">build.gradle</span> file:</p>

<pre class="">compile group: 'org.apache.httpcomponents', name: 'httpclient', version: '4.5.2'</pre>
<p style="text-align: justify;">Also, for reference, here's my gradle version:</p>

<pre class="lang:default mark:4 decode:true">C:\Users\manthanhd\Documents\projects\gradle-lint-test&gt;gradle -v

------------------------------------------------------------
Gradle 2.12
------------------------------------------------------------

Build time:   2016-03-14 08:32:03 UTC
Build number: none
Revision:     b29fbb64ad6b068cb3f05f7e40dc670472129bc0

Groovy:       2.4.4
Ant:          Apache Ant(TM) version 1.9.3 compiled on December 23 2013
JVM:          1.7.0_10 (Oracle Corporation 23.6-b04)
OS:           Windows 8 6.2 amd64</pre>
<p style="text-align: justify;">I ran the gradle clean build command to assess the initial size of the build. As expected, the war file was massive at <strong>14.3 megabytes</strong>. For a small project that has a simple healthcheck endpoint, this is huge. So, to streamline this, I followed the <a href="https://github.com/nebula-plugins/gradle-lint-plugin/wiki/Using-Lint" target="_blank">gradle-lint-plugin guide</a> to use and apply the plugin. I added some rules of my own too. Here's what my <span class="lang:default decode:true crayon-inline">build.gradle</span> file looks like:</p>

<pre class="lang:default mark:4,11-18,32 decode:true ">buildscript {
    repositories {
        mavenCentral()
        jcenter()
    }
    dependencies {
        classpath("org.springframework.boot:spring-boot-gradle-plugin:1.3.5.RELEASE")
    }
}

plugins {
    id 'nebula.lint' version '0.30.12'
}

allprojects {
    apply plugin: 'nebula.lint'
    gradleLint.rules = ['all-dependency', 'unused-dependency', 'unused-exclude-by-dep'] // add as many rules here as you'd like
}

apply plugin: 'java'
apply plugin: 'eclipse'
apply plugin: 'idea'
apply plugin: 'spring-boot'

jar {
    baseName = 'gs-spring-boot'
    version =  '0.1.0'
}

repositories {
    mavenCentral()
    jcenter()
}

sourceCompatibility = 1.7
targetCompatibility = 1.7

dependencies {
    compile group: 'org.apache.httpcomponents', name: 'httpclient', version: '4.5.2'
    // tag::jetty[]
    compile("org.springframework.boot:spring-boot-starter-web") {
        exclude module: "spring-boot-starter-tomcat"
    }
    compile("org.springframework.boot:spring-boot-starter-jetty")
    // end::jetty[]
    // tag::actuator[]
    compile("org.springframework.boot:spring-boot-starter-actuator")
    // end::actuator[]
    testCompile("junit:junit")
}

task wrapper(type: Wrapper) {
    gradleVersion = '2.1'
}</pre>
<p style="text-align: justify;">Once that was out of the way, I simply ran <span class="lang:default decode:true crayon-inline ">gradle clean build</span> command.</p>

<pre class="lang:default decode:true ">C:\Users\manthanhd\Documents\projects\gradle-lint-test&gt;gradle clean build
:clean
:compileJava
:processResources UP-TO-DATE
:classes
:findMainClass
:jar
:bootRepackage
:assemble
:compileTestJava UP-TO-DATE
:processTestResources UP-TO-DATE
:testClasses UP-TO-DATE
:test UP-TO-DATE
:check UP-TO-DATE
:build
:lintGradle

This project contains lint violations.  A complete listing of the violations follows.
Because none were serious, the build's overall status was unaffected.

warning   unused-dependency                  one or more classes in org.springframework:spring-web:4.2.6.RELEASE are required by your code directly

warning   unused-dependency                  one or more classes in org.springframework.boot:spring-boot:1.3.5.RELEASE are required by your code directly

warning   unused-dependency                  one or more classes in org.springframework.boot:spring-boot-autoconfigure:1.3.5.RELEASE are required by your code directly

warning   unused-dependency                  one or more classes in org.springframework:spring-context:4.2.6.RELEASE are required by your code directly

warning   unused-dependency                  one or more classes in org.springframework:spring-web:4.2.6.RELEASE are required by your code directly

warning   unused-dependency                  one or more classes in org.springframework.boot:spring-boot:1.3.5.RELEASE are required by your code directly

warning   unused-dependency                  one or more classes in org.springframework.boot:spring-boot-autoconfigure:1.3.5.RELEASE are required by your code directly

warning   unused-dependency                  one or more classes in org.springframework:spring-context:4.2.6.RELEASE are required by your code directly

warning   unused-dependency                  this dependency is unused and can be removed
build.gradle:39
compile group: 'org.apache.httpcomponents', name: 'httpclient', version: '4.5.2'

warning   unused-dependency                  this dependency is unused and can be removed
build.gradle:39
compile group: 'org.apache.httpcomponents', name: 'httpclient', version: '4.5.2'

? build.gradle: 10 problems (0 errors, 10 warnings)

To apply fixes automatically, run fixGradleLint, review, and commit the changes.


BUILD SUCCESSFUL

Total time: 15.742 secs</pre>
<p style="text-align: justify;">Excellent! It found the unused dependencies. I was quite pleased with this result so I ran <span class="lang:default decode:true crayon-inline">gradle fixGradleLint</span> command. According to the wiki, this command should fix the issues that it found by making changes to the <span class="lang:default decode:true crayon-inline">build.gradle</span> file. Unfortunately, this didn't work as it resulted in an error.</p>

<pre class="lang:default mark:8 decode:true ">C:\Users\manthanhd\Documents\projects\gradle-lint-test&gt;gradle fixGradleLint
:fixGradleLint FAILED

FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':fixGradleLint'.
&gt; com.netflix.nebula.lint.jgit.api.errors.PatchApplyException: Cannot apply: HunkHeader[36,7-&gt;36,14]

* Try:
Run with --stacktrace option to get the stack trace. Run with --info or --debug option to get more log output.

BUILD FAILED

Total time: 10.808 secs</pre>
<p style="text-align: justify;">I've already raised a github issue to draw their attention to this problem. For information on progress, feel free to watch or add to the <a href="https://github.com/nebula-plugins/gradle-lint-plugin/issues/38" target="_blank">issue</a>.</p>
<p style="text-align: justify;">So, even if the automated fix didn't work, I wasn't done yet. I looked around in the <span class="lang:default decode:true crayon-inline ">build</span> directory looking for something that I can use to apply for the fix myself. After all, since <span class="lang:default decode:true crayon-inline ">gradle lintGradle</span> and <span class="lang:default decode:true crayon-inline ">gradle fixGradleLint</span> are two separate commands, independent of each other, it must store some form of state somewhere. Fortunately I found it. It stores its state in a file called <span class="lang:default decode:true crayon-inline">lint.patch</span> directly under the <span class="lang:default decode:true crayon-inline">build</span> directory at the root of your project.</p>
<p style="text-align: justify;">Hurrah! I quickly opened it. It was a normal git patch file. While looking at it, I was slightly confused with the additions that it was making. Here's what mine looked like:</p>

<pre class="">diff --git a/build.gradle b/build.gradle
--- a/build.gradle
+++ b/build.gradle
@@ -36,7 +36,14 @@
 targetCompatibility = 1.7
 
 dependencies {
+   compile 'org.springframework:spring-web:4.2.6.RELEASE'
+   compile 'org.springframework.boot:spring-boot:1.3.5.RELEASE'
+   compile 'org.springframework.boot:spring-boot-autoconfigure:1.3.5.RELEASE'
+   compile 'org.springframework:spring-context:4.2.6.RELEASE'
+   compile 'org.springframework:spring-web:4.2.6.RELEASE'
+   compile 'org.springframework.boot:spring-boot:1.3.5.RELEASE'
+   compile 'org.springframework.boot:spring-boot-autoconfigure:1.3.5.RELEASE'
+   compile 'org.springframework:spring-context:4.2.6.RELEASE'
-    compile group: 'org.apache.httpcomponents', name: 'httpclient', version: '4.5.2'
     // tag::jetty[]
     compile("org.springframework.boot:spring-boot-starter-web") {
         exclude module: "spring-boot-starter-tomcat"</pre>
<p style="text-align: justify;">Looking at that file, I could immediately tell that it was adding some compile time dependencies and was removing one httpcomponents dependency. While I agree with the removal of the httpcomponents dependency, I was slightly confused why it wasn't replacing the existing spring-boot dependencies with the ones it was adding. It should've replaced them because having both provides no benefit. What its trying to add is more specific than the dependencies that are already there! To prove myself right, I removed all the existing <span class="lang:default decode:true crayon-inline ">compile</span> dependencies (expect <span class="lang:default decode:true crayon-inline ">testCompile</span> ones) and added the ones that it was going to add from the patch file. Here's what the dependency section of my <span class="lang:default decode:true crayon-inline ">build.gradle</span> looked like:</p>

<pre class="">dependencies {
    compile 'org.springframework:spring-web:4.2.6.RELEASE'
    compile 'org.springframework.boot:spring-boot:1.3.5.RELEASE'
    compile 'org.springframework.boot:spring-boot-autoconfigure:1.3.5.RELEASE'
    compile 'org.springframework:spring-context:4.2.6.RELEASE'
    compile 'org.springframework:spring-web:4.2.6.RELEASE'
    compile 'org.springframework.boot:spring-boot:1.3.5.RELEASE'
    compile 'org.springframework.boot:spring-boot-autoconfigure:1.3.5.RELEASE'
    compile 'org.springframework:spring-context:4.2.6.RELEASE'
    /*compile group: 'org.apache.httpcomponents', name: 'httpclient', version: '4.5.2'
    // tag::jetty[]
    compile("org.springframework.boot:spring-boot-starter-web") {
        exclude module: "spring-boot-starter-tomcat"
    }
    compile("org.springframework.boot:spring-boot-starter-jetty")
    // end::jetty[]
    // tag::actuator[]
    compile("org.springframework.boot:spring-boot-starter-actuator")*/
    // end::actuator[]
    testCompile("junit:junit")
}</pre>
<p style="text-align: justify;">I quickly ran <span class="lang:default decode:true crayon-inline">gradle clean build</span> and sure enough, this time the build size had shrunk to almost a third of its original size - <strong>5.5 megabytes</strong>! Also, the <span class="lang:default decode:true crayon-inline ">lintGradle</span> task at the end of the build didn't report any errors.</p>
<p style="text-align: justify;">I can certainly see the value of this plugin in a continuous integration system as unused libraries cause host of memory related issues like lack of permgen space in java web containers. Also, its better to let developers focus on core development and let the periphery issues such as these be handled by an automated plugin like this one.</p>
<p style="text-align: justify;">I hope the developers on the github project fix the issues related to the <span class="lang:default decode:true crayon-inline ">fixGradleLint</span> task soon.</p>
<p style="text-align: justify;">Check out the source for this test on <a href="https://github.com/manthanhd/gradle-lint-test" target="_blank">github</a>.</p>