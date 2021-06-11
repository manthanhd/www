---
id: 346
title: Notes from AWS Developer Training (Day Three)
date: 2016-03-02T22:24:16+00:00
author: Manthan Dave
layout: post
guid: https://www.manthanhd.com/?p=346
permalink: /2016/03/02/notes-from-aws-developer-training-day-three/
categories:
  - Educational
tags:
  - auto scaling
  - aws
  - cloud
  - ec2
  - elasticache
  - iam
  - notes
  - security
---
<h3 style="text-align: justify;">Creating Serverless Projects</h3>
<p style="text-align: justify;">In a server less environment, Amazon Lambda can be used in conjunction with Amazon API Gateway for HTTP interfacing, Amazon S3 for storage, Amazon ElastiCache for caching and DynamoDB/RDS for database storage. Checkout the Servless Application Framework at <a href="http://serverless.com">serverless.com</a> for more info.</p>

<h2 style="text-align: justify;">Securing data in AWS</h2>
<p style="text-align: justify;">Infrastructure should be treated as code, I.e. Version control systems. Automate security and increase testing frequency via CI/CD. Fail early and fast. Test at production scale. No need to keep the test servers alive. Spin up the entire production environment in test, deploy the code, run the tests and then tear down the environment.<!--more--></p>
<p style="text-align: justify;">DevOps provides efficiency that speeds up the development lifecycle (build &gt; test &gt; release &gt; monitor &gt; plan). Validate security in each step to avoid slowing down the whole lifecycle.</p>
<p style="text-align: justify;">Amazon’s shared responsibility model.</p>
<p style="text-align: justify;">How do we get certificates for the internal private DNS for an AWS EC2 instance?</p>
<p style="text-align: justify;">All requests to AWS must be signed using access key ID and secret access key. If using the SDK, this is done automatically. However, if this is done manually, use signature version 4 to sign requests.</p>
<p style="text-align: justify;">Support v2 and v4, most services support v4. V4 is more secure as it derives a key from secret access key and then uses that derived key to sign requests. V2 on the other hand does this very plainly as it uses the secret key directly to sign requests.</p>
<p style="text-align: justify;">One way to provide permissions is to add users to a group that has policies attached to it. All users in that group have permissions assigned to them as long as they are in that group. Drawback is that this is a static set of permissions. As in if two developers are out on holiday for  weeks and only one is available, all three developers still have all the access. Also if one of the developers need access to EMR, and if policy is changed to include that permission, all the developers, whether or not they need it, inherit that permission.</p>
<p style="text-align: justify;">Better way to do this is to use IAM roles. Create a role called EMR with the policy. The user that needs permission assumes that role. This provides that user with a new set of access key and secret key that has access to EMR.</p>
<p style="text-align: justify;">When authenticating with SSO or AD, the authenticating application checks if the user is in the right group to access the target application. If the user isn't in the right group then it must check if there are roles that can be assumed to access the target application. If the roles exist, the authenticating application then checks if the user is able or allowed to assume any of those roles. If the user is allowed, the application assumes the role for him, granting access. One of the ways this can be done is by using SAML assertion with IAM.</p>

<h2 style="text-align: justify;">Getting started with IAM</h2>
<p style="text-align: justify;">Once you have the root account, create a general IAM account for yourself. Assign permissions that you need to yourself. If a admin is needed, create a group called Admins and then assign basic admin permissions to that role. When a new admin joins, he/she can be added to Admins group. Give permissions to admins group as needed.</p>
<p style="text-align: justify;">For a developer who joins the team, again create a Developers group and assign permissions to that group as needed. When a special permission is needed to access a specific thing, create a role for that and allow people to assume roles for a limited time or as long as they need it for. For temporary contractors and consultants, assign one permission called Assume Role. This allows them to temporarily assume roles when needed.</p>
<p style="text-align: justify;">The assume role permission can be restricted by IP, as in you’re only allowed to assume a role when you're on site.</p>
<p style="text-align: justify;">For authentication, AWS management console needs username and password. Other tools like AWS CLI, SDKs and API queries need access key and secret keys.</p>
<p style="text-align: justify;">NEVER USE ROOT ACCOUNT CREDENTIALS ANYWHERE.</p>
<p style="text-align: justify;">Two types of IAM permission types. User centric where each user is given access to specific resources. Second type of permission type is resource centric where specific access to a resource is opened up to a set of users.</p>
<p style="text-align: justify;">Use AWS provided library of pre-defined Policy Templates or AWS policy generator. Preferably, create policies in object oriented way using AWS Cloud Formation or AWS SDKs.</p>
<p style="text-align: justify;">IAM policy rule procedure is to deny access by default until access is explicitly allowed in all policies. If something is explicitly denied, it can never be allowed. Explicit deny cannot be overwritten.</p>
<p style="text-align: justify;">Might be possible to restrict users from uploading content above a certain size.</p>
<p style="text-align: justify;">For external users, use OpenID Connect for authentication with external services like Google/Facebook/Twitter login along with IAM for authorisation.</p>

<h2 style="text-align: justify;">Caching</h2>
<p style="text-align: justify;">CloudFront caches content based on edge location. If content is available, it will serve from its edge location, if not, it will serve from S3.</p>
<p style="text-align: justify;">Each caching behaviour can be configured. Params like Path Pattern to be cached, request origin to forward to, if forward URLs should have query strings in them etc.</p>
<p style="text-align: justify;">Path patterns max is 255 chars. Case sensitive. Eg. /*.jpg means cache all JPG files.</p>
<p style="text-align: justify;">With regards to query strings, cache behaviour will differ based on value of query strings in the request. Example: /hello?query1=a and /hello?query2=b will result in different cache copies served. Cache can also serve different URLs based on different request header values or cookies.</p>
<p style="text-align: justify;">The time to live (TTL) value controls how long the value should stay in the cache. Longer TTLs for static content, short for dynamic.</p>
<p style="text-align: justify;">A custom error page page can be configured to be serve if the cache fails to find the origin content (if origin changed but cache didn't for some reason and its expecting the file to be present on origin).</p>
<p style="text-align: justify;">Authentication works via public/private key pair. CloudFront has the public key while the application has the private key. The URLs must be signed before sending to cloud front. The application signs using its private key, sends URL to cloud front. If the URL is valid, cloud front serves the content.</p>
<p style="text-align: justify;">User sessions can be stored in DynamoDB or ElastiCache, depending on need.</p>
<p style="text-align: justify;">ElastiCache runs, underneath the covers, me cache or redis cluster. Provides managed service that include patching and backup.</p>
<p style="text-align: justify;">Memcache is known for high performance and scale. Allows running of large nodes with multiple cores or threads. Ability to scale in or out. Data can be partitioned across multiple shards. One downside is that the data model is very simple.</p>
<p style="text-align: justify;">Redis is quite similar to Memcache where it's open source and in memory. Redis can only scale vertically (check!!). Has simple master/slave model, not shards (check!!). Comes with complex data types. Has pub/sub capability - the client being informed of events on the server. Not very good for multi-threaded performance (check!!).</p>
<p style="text-align: justify;">Cache is loaded by lazy loading as in data is only loaded when required.</p>

<h2 style="text-align: justify;">CloudWatch</h2>
<p style="text-align: justify;">One can use CloudWatch agent to aggregate logs from EC2 instances to the central CloudWatch server. To do this, install the cloudwatch agent on the EC2 instance.</p>
<p style="text-align: justify;">From any log streams, custom cloudwatch metric can be created. Once the metric is created, an alarm can be configured.</p>
<p style="text-align: justify;">Cloud watch alarms can call HTTP endpoints, like calling web hooks to trigger different kind of behaviours based on the event.</p>
<p style="text-align: justify;">Cloud watch metrics can be aggregated on the server side by providing params like start and end times, period and metric names.</p>
<p style="text-align: justify;">CloudWatch dashboards can be used to provide a bird's eye view of all metrics pertaining to a specific application. It also allows adding of text widgets which one can use to provide some free text or images.</p>
<p style="text-align: justify;">JSON path expression can be used to search logs within CloudWatch. This style of expression can also be used to extract values from logs. Once values are extracted, they can be used to create a metric. For instance, use Regex to extract response times from tomcat logs, create a CloudWatch metric out of it called Latency and then set a CloudWatch alarm that fires off an email when latency exceeds 2 seconds.</p>
<p style="text-align: justify;">CloudWatch can be integrated into other services like Elasticsearch and Kibana. It can also work with AppDynamics or New Relic.</p>

<h2 style="text-align: justify;">EC2 and Auto scaling</h2>
<p style="text-align: justify;">EC2 instances can be launched with a script that is executed upon launch. This script or piece of code is called user data.</p>
<p style="text-align: justify;">When architecting a VPC, make sure that the auto scaling group is spread across two availability zones and each logical component (database server or application server) is in its own private subnet.</p>
<p style="text-align: justify;">Auto scaling can scale applications based on cloud watch metrics or time. Time based works like… Scaling out on Monday morning and scaling in on Friday evenings.</p>
<p style="text-align: justify;">Auto scaling forms two parts: Launch configuration and Amazon Machine Image (AMI). Launch configuration tells it how to launch something and what to do when it's launched. AMI tells it what to launch. AMI is in three categories: Bare (nothing but base image), Silver (optimised based OS image with prerequisites (Java/Node) installed, Gold (Fully fledged application with application code and prerequisites.</p>
<p style="text-align: justify;">If one wants the deployment to fail if something in user data fails, then one needs to initiate the cfn-signal with -e option specifying the error code. One can also use cfn-in it to initialise the script. This attribute in cloud formation will do roll back on its own without the sub-script having to call cfn-signal.</p>