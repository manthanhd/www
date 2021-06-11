---
id: 702
title: Documenting project information with Maven
date: 2018-03-12T09:45:24+00:00
author: Manthan Dave
layout: post
guid: https://www.manthanhd.com/?p=702
permalink: /2018/03/12/documenting-project-information-with-maven/
categories:
  - Educational
  - Findings
  - Thoughts
tags:
  - java
  - maven
---
In most projects I've worked on, the project information is captured in some sort of `README.md` file. While this has its strengths, such as ability to write free form text and embed images, it doesn't quite fit with the project because when the artifact is produced during the build time, the read me is not checked into the artifact repository along with it. So how do I know who the contributors were for that specific artifact version? How do I know where the project was hosted at the time?

Well, today I learned that maven provides ability to capture some of those attributes and some more! I like this because it means that when the artifact gets checked in (along with its pom.xml) the details about the project are captured for that version, frozen in time.

Great thing about these attibutes is that when you run <code>mvn site</code> they are used in the project information html page that maven produces. Technically, then you can upload this to any statically hosted site location, all versioned up and ready to be explored.

Ok so lets begin exploring these attributes. The first three attributes are the basic ones:
<pre class="lang:xhtml decode:true">&lt;name&gt;Sample API (HTTP)&lt;/name&gt;
&lt;description&gt;HTTP API endpoint&lt;/description&gt;
&lt;url&gt;https://github.com/manthanhd/sample-api&lt;/url&gt;</pre>
For people working in corporations and companies, the following might be useful, especially when open sourcing the project:
<pre class="lang:xhtml decode:true crayon-selected">&lt;organization&gt;
  &lt;name&gt;Company Name Inc&lt;/name&gt;
  &lt;url&gt;https://www.company.org&lt;/url&gt;
&lt;/organization&gt;</pre>
Additionally, the <code>licenses</code> tag can be used to describe some licensing information. How many times have you come across a project that has changed the licensing information half way through versions? This is a life saver (at least legally)!
<pre class="lang:xhtml decode:true ">&lt;licenses&gt;
  &lt;license&gt;
    &lt;name&gt;MIT License&lt;/name&gt;
    &lt;url&gt;https://opensource.org/licenses/MIT&lt;/url&gt;
  &lt;/license&gt;
&lt;/licenses&gt;</pre>
Some source control information is also useful, however, whats more useful is knowing where to go in order to raise issues. In most companies, people use source control to store code but then use an alternative mechanism like Jira or Trello to mange issues. The <code>scm</code> and <code>issueManagement</code> tags are useful here in clarifying such information:
<pre class="lang:xhtml decode:true">&lt;scm&gt;
  &lt;url&gt;https://username@company-bitbucket.org:orgname/sample-api&lt;/url&gt;
  &lt;connection&gt;scm:git:git://username@company-bitbucket.org:orgname/sample-api.git&lt;/connection&gt;
  &lt;developerConnection&gt;scm:git:git@company-bitbucket.org:orgname/sample-api.git&lt;/developerConnection&gt;
&lt;/scm&gt;

&lt;issueManagement&gt;
  &lt;url&gt;https://some-jira.companyname.org/newissue&lt;/url&gt;
  &lt;system&gt;Private Jira powered issue management&lt;/system&gt;
&lt;/issueManagement&gt;</pre>
Developer information on projects is very useful. Companies don't usually like this because, well, developers are supposed to be expendable, however, in my humble opinion, it is useful to list developers who were working on the project at the time. This way the versions can be tracked, at least to the lead working on it at the time. Use it thusly:
<pre class="lang:xhtml decode:true">&lt;developers&gt;
  &lt;developer&gt;
    &lt;name&gt;John Doe&lt;/name&gt;
    &lt;email&gt;john.doe@company.com&lt;/email&gt;
    &lt;roles&gt;
      &lt;role&gt;Founder&lt;/role&gt;
      &lt;role&gt;Chief Engineer&lt;/role&gt;
    &lt;/roles&gt;
  &lt;/developer&gt;
&lt;/developers&gt;</pre>
For contributors, people who have worked on the project, although not necessarily part of it exclusively, use:
<pre class="lang:xhtml decode:true">&lt;contributors&gt;
  &lt;contributor&gt;
    &lt;name&gt;Joe Bloggs&lt;/name&gt;
    &lt;email&gt;joe.bloggs@company.com&lt;/email&gt;
    &lt;roles&gt;
      &lt;role&gt;Occasional Committer&lt;/role&gt;
    &lt;/roles&gt;
  &lt;/contributor&gt;
&lt;/contributors&gt;</pre>
Lastly, it is useful to capture some information about pipeline that was used to create the project. For this, maven has <code>ciManagement</code> tag:
<pre class="lang:xhtml decode:true">&lt;ciManagement&gt;
  &lt;url&gt;https://company-jenkins.org:orgname/sample-api/pipeline&lt;/url&gt;
  &lt;system&gt;Some Jenkins based pipeline&lt;/system&gt;
&lt;/ciManagement&gt;</pre>
The above is just a small example of what you can do with these attributes, there are many more sub-attributes that you can explore. Hope this is a good introduction and kicks off some ideas of what you can do with this information.