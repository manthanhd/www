---
id: 869
title: Selectively running a TestNG suite using maven
date: 2020-09-12T16:28:41+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2020/09/12/787-revision-v1/
permalink: /?p=869
---
<!-- wp:paragraph -->
<p>Consider a setup where there are two packages, package <code>A</code> and package <code>B</code>, each with a distinct set of tests. Lets assume that that the tests in both packages are mutually exclusive, i.e. only one of them can be run at a time, running both would cause issues, perhaps because they rely on a common resource (bad practice but lets go with it for now).</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>In this case, the testng configuration is comprised of two xml files, each describing a suite. Here's <code>src/test/resources/testng-suite-a.xml</code> describing a suite for package <code>A</code>:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&lt;?xml version = "1.0" encoding = "UTF-8"?> 
&lt;!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd" >
&lt;suite name = "Suite A"> 
&lt;test name = "Tests for doing A, N times."> 
&lt;packages> 
&lt;package name="com.test.a"/> 
&lt;/packages> 
&lt;/test> 
&lt;/suite></code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Here the suite will only run tests that are within package <code>com.test.a</code>. Similarly, here's <code>src/test/resources/testng-suite-b.xml</code> describing a suite for package <code>B</code>:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&lt;?xml version = "1.0" encoding = "UTF-8"?> 
&lt;!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd" >
&lt;suite name = "Suite B"> 
&lt;test name = "Tests for doing B, N times."> 
&lt;packages> 
&lt;package name="com.test.b"/> 
&lt;/packages> 
&lt;/test> 
&lt;/suite></code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>Evidently this will only run tests that are within package <code>com.test.b.*</code>. Because there are two suites, surefire needs to know which one to run. Statically, this can be configured like so:<br></p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"><pluginmanagement>
  <plugins>
      <plugin>
      <groupid>org.apache.maven.plugins</groupid>
      <artifactid>maven-surefire-plugin</artifactid>
      <version>2.20</version>
      <configuration>
          <suitexmlfiles>
              <suitexmlfile>${project.basedir}/src/test/resources/testng-suite-a.xml</suitexmlfile>
          </suitexmlfiles>
      </configuration>
    </plugin>
  </plugins>
</pluginmanagement></pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>With this configuration, maven will run the tests in suite a by default. To allow the "switching", simplest approach is to use maven properties. Extract the <code>${project.basedir}/src/test/resources/testng-suite-a.xml</code> into a maven property and replace the literal with the property reference. This will look like so:</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"><properties>
  <suitefile>${project.basedir}/src/test/resources/testng-suite-a.xml</suitefile>
</properties>
...
<configuration>
    <suitexmlfiles>
        <suitexmlfile>${suiteFile}</suitexmlfile>
    </suitexmlfiles>
</configuration></pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>Still, by default maven will run the tests in suite a by default. The difference is that since the suite file is in maven properties, it can be overridden using <code>-D</code> options.</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>mvn test # Will run suite a by default
mvn test -DsuiteFile=src/test/resources/testng-suite-b.xml # Runs suite b</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p><br></p>
<!-- /wp:paragraph -->