---
id: 361
title: 'AppDynamics Power User Training Notes &#8211; Day Two'
date: 2016-03-23T23:35:55+00:00
author: Manthan Dave
layout: post
guid: https://www.manthanhd.com/?p=361
permalink: /2016/03/23/appdynamics-power-user-training-notes-day-two/
categories:
  - Educational
tags:
  - appdynamics
  - monitoring
  - notes
---
<p style="text-align: justify;">Health rules are conditions that if fulfilled relate to good health for a particular node. The conditions can be based on load (calls/min), response times (x ms average) or error rate. AppDynamics provides 7 health rules out of the box. When one or many health rules are violated, an alert is issued. Depending on the integrations configured, this alert can be an email or SMS.</p>
<p style="text-align: justify;">Health rule can be based on transaction performance or node health. In case of transaction performance, it can target specific transactions or all transactions in a tier. However, in case of node heath, this can target specific node or nodes in a tier or nodes by type.</p>
<p style="text-align: justify;">The data that health rule uses can also be configured. This can be a time period that it should use. This can be in free form numeric minutes. One can also specify wait time after a violation, i.e. how long should it wait to reassess health rule after it has been violated. This should be long enough so that it has enough new data to positively reassess the condition.<!--more--></p>
<p style="text-align: justify;">Diagnostic session is a time period in which AppDynamics collects extra snapshots for one or more business transactions. All transactions have full call graphs. Diagnostic sessions are normally used after a health rule violation.</p>
<p style="text-align: justify;">Diagnostic sessions can be policy based where it is automatically executed as soon as a particular health rule violation has happened. Depending on the situation, the policy can then have actions where it can send an email or a HTTP request with event details.</p>
<p style="text-align: justify;">Development mode is where it creates snapshots where it creates transactions for every business transaction. Highly advised that it is only used in testing environments as it has more overhead compared normal production mode. It has safeguards built in where it automatically turns itself off to production mode if 500 calls/min or 90% heap utilisation happens.</p>
<p style="text-align: justify;">All HTTP error codes from 400 to 505 are treated as errors. Also, all exceptions within JVM that are handled but logged as fatal using Log4J or java.util.logging are also treated as errors.</p>
<p style="text-align: justify;"><b>Scenario</b>: Customer calls regarding an error that they saw on the page.</p>
<p style="text-align: justify;"><b>Solution</b>: Start a diagnostic session or periodic collection or brief development mode to get snapshots with full call graphs. Go to application dashboard and on the right pane, click errors. Or, go to snapshots and filter by errors. Once you locate the snapshot with errors that has full call graph (blue document icon next to it),  double click on it to view and go to error details. Make sure you turn off the development mode.</p>
<p style="text-align: justify;">A business transaction is detected at the point when it enters the application. If a transaction or node is not detected, it can manually be added. Configuration &gt; Instrumentation &gt; Transaction detection. In the custom rules section, a custom rule for detecting a transaction can be added.</p>
<p style="text-align: justify;">Exit point is a call that leaves node or environment either to a new monitored environment or a tier. Example database/mail server etc.</p>
<p style="text-align: justify;">Use Data Collectors to collect data that's passed into methods or payloads. Its available in Application &gt; Configuration &gt; Instrumentation &gt; Data Collectors tab. For methods, use getter chain and specify object getter to invoke which is likely to provide data that's needed. For instance, to collect movie title from an object that’s passed into search(movieObject) method, if the getter chain is getTitle() then the full invocation will be movieObject.getTitle().</p>