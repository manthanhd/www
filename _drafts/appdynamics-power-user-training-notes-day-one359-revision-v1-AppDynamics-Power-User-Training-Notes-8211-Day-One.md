---
id: 422
title: 'AppDynamics Power User Training Notes &#8211; Day One'
date: 2016-08-27T13:23:10+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2016/08/27/359-revision-v1/
permalink: /?p=422
---
<p style="text-align: justify;">Node is mapped to individual JVM or CLR application in environment. If an environment has multiple JVMs running then each maps to a node.</p>
<p style="text-align: justify;">Tier is a logical piece of application. For example a piece of functionality. Each web server has node. A single tier can span multiple web servers and each of those web servers can contain multiple nodes.</p>
<p style="text-align: justify;">Application traffic is organised into business transactions. Each transaction is a distinct user activity like Login or register etc. When a request comes in for the first time, it is tagged with a GUID and that GUID is tracked across the environment. Requests with similar patterns are grouped together. This group is then given a default name. This is default name is based on how the application is designed.</p>
<p style="text-align: justify;">Since each business transaction works across nodes, each has its own flow map.<!--more--></p>
<p style="text-align: justify;">A business transaction is a specific user activity. It represents a flow of data across the application. A tier, on the other hand represents a static, logical set of services.</p>
<p style="text-align: justify;">Transaction snapshot is a static state of a transaction at any given time. It collects detailed information regarding one business transaction at any given time.</p>
<p style="text-align: justify;">Periodic snapshot is a snapshot that’s taken once every 10 minutes. This is taken regardless of any errors occurring within the application.</p>
<p style="text-align: justify;">Slow and error transaction snapshot is a snapshot that is taken when calls happening below a certain threshold or when errors are happening above a certain error rate.</p>
<p style="text-align: justify;">Snapshots can be archived. Once a snapshot is archived, it is permanently visible in the UI and won’t get deleted with periodic cleanup. A transaction that has been archived cannot be unarchived and must be deleted if archive is no longer needed.</p>

<h3 style="text-align: justify;">AppDynamics architecture</h3>
<p style="text-align: justify;">App server agent. This could be .NET, Java, PHP etc. This is mandatory.</p>
<p style="text-align: justify;">Machine agent is optional. Collects raw metrics regarding the machine that it is on.</p>
<p style="text-align: justify;">AppDynamics supports role based authentication. Also supports other integrations like LDAP/SAML etc.</p>
<p style="text-align: justify;">AppDynamics controller never actively initiates connection to an agent. The agent connects to the AppDynamics controller which then responds with any additional requests.</p>
<p style="text-align: justify;">Baseline is a set of values calculated from metrics within a time range. We could capture “ideal” scenario by recording a baseline when application was running smoothly. Then at a later date, when application isn’t performing optimally, one can compare the current performance with the historic “ideal” baseline to identify problems.</p>
<p style="text-align: justify;">Baseline can also be a dynamic, rolling baseline where, for example, every monday 2pm, baseline is taken. Now next monday at 2pm metrics are compared to the baseline that was taken on the previous monday. This is a dynamic baseline so if the current performance peaks, the baseline will adjust itself to compensate for the peak.</p>
<p style="text-align: justify;">When a baseline is defined, if the deviation of the current data is certain level above the baseline, AppDynamics will automatically raise an alarm indicating that.</p>
<p style="text-align: justify;">Rather than deleting a transaction, it is better to exclude it as restoring a deleted transaction is much harder (needs re-creating) than restoring a excluded one.</p>