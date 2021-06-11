---
id: 420
title: 'AppDynamics Power User Training Notes &#8211; Day Three'
date: 2016-08-27T13:22:42+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2016/08/27/364-revision-v1/
permalink: /?p=420
---
<p style="text-align: justify;">Dashboards. Useful for displaying and sharing information across teams or departments. Allows integrating other tools via iFrame widget. Dashboards can also be embedded into other services or even web pages.</p>
<p style="text-align: justify;">A dashboard can also be converted into a report. One can also schedule periodic emailing of reports to a recipient list. This might be useful to create a “weekly/monthly digest” of data relating to an environment.</p>
<p style="text-align: justify;">Policy triggers define events that trigger actions. When the criteria in a policy is met, a corresponding trigger is activated. This can be used to control behaviour such as increaing thread pool size as load increases etc. The trigger must be associated with a health rule violation. For example, if a health rule specifies “calls higher than normal” then a policy can be created based on this heath rule. The trigger gets executed when violation of the specified health rule occurs. Triggers can span different stages of health rule violations, for example, start and/or end. More than one health rule violation event can be specified.<!--more--></p>
<p style="text-align: justify;">When defining actions for policy triggers, one can configure the action to require human approval before executing a particular action. In this case, one must specify email address of the approver within the action.</p>
<p style="text-align: justify;">Runbook automation is a remediation action that is executed on a node on a policy trigger. For instance, if for some reason there’s a java out of memory error, the runbook automation can backup the logs and restart the JVM. Everything that the runbook automation does is logged with the action that was executed, time and the local script that the automation ran to carry out the specified action. There’s three types:</p>
<p style="text-align: justify;"><b>Remediation action</b>: Run arbitrary script on a node</p>
<p style="text-align: justify;"><b>HTTP request</b>: Make a HTTP request to some server notifying it of the issue.</p>
<p style="text-align: justify;"><b>Cloud auto-scaling</b>: Scale out or scale in on cloud services like AWS.</p>