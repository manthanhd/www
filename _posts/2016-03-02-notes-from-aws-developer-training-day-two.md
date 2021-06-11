---
id: 340
title: Notes from AWS Developer Training (Day Two)
date: 2016-03-02T22:21:03+00:00
author: Manthan Dave
layout: post
guid: https://www.manthanhd.com/?p=340
permalink: /2016/03/02/notes-from-aws-developer-training-day-two/
categories:
  - Educational
tags:
  - aws
  - cloud
  - kinesis
  - lambda
  - notes
  - sns
  - sqs
  - swf
---
<h2 style="text-align: justify;">Achieving loose coupling with Events</h2>
<p style="text-align: justify;">Amazon SQS, SNS, DynamoDB Streams, Kinesis Streams, Lambda.</p>
<p style="text-align: justify;">With the event driven architecture, two systems don't need to know about each other. Each of them can fire events while the other responds to that specific event.</p>
<p style="text-align: justify;">SNS has publish/subscribe model. When publisher pushes, all subscribers immediately get the message. This can be email, SMS, SQS, Lambda etc.</p>
<p style="text-align: justify;">SQS queuing for delivery method. Messages are persisted until they are polled. Extremely scalable. Can potentially contain millions of messages.<!--more--></p>
<p style="text-align: justify;">SNS and SQS can be used together. SNS delivers to the SQS queues each for one EC2 instance who then carry out a specific task. On push, SQS provides no guarantee for ordering.</p>
<p style="text-align: justify;">DynamoDB streams allow firing of events in a stream. For example, on image upload, the image is sent to S3 and the metadata is stored in DynamoDB. When the metadata is stored, DynamoDB fires off event in a stream which is read by Lambda. This can then do some computation around the data coming from DynamoDB.</p>
<p style="text-align: justify;">Amazon Kinesis Streams is a fully managed streaming data service. Helps aggregate data. Data stored in a Kinesis stream is ordered. Client can read a small segment from the stream. Allows adding of checkpoints so that if the client that is reading from a stream fails, it can continue from the last checkpoint.</p>
<p style="text-align: justify;">Lambda is a event handler. Not a replacement but a variation of event driven server less code.</p>

<h2 style="text-align: justify;">Kinesis</h2>
<p style="text-align: justify;">Offers services to load and process large amounts of data. Allows building of streaming data applications.</p>
<p style="text-align: justify;">First thing that is to be created in Kinesis is a stream. Data records are put into the stream which are then read out from the other side. Stream has shards. Each shard has 1 MBPS write and 100 MBPS. Reads 5 transactions per second and writes are 1000 records per second. Shards can be merged afterwards if they are not in use. This action is reversible.</p>
<p style="text-align: justify;">Each data record has three parts. Sequence number (automatically created), partition key and then the data to be streamed. Data can be JSON/XML/Binary etc. The partition key determines which shard gets the data.</p>
<p style="text-align: justify;">Operations on stream: Create/List/Delete. Operations on shard: Retrieve/Reshard (increase or merge shard count)/Change data retention period. Every data record has its own expiration time.</p>
<p style="text-align: justify;">When data is inserted, the sequence number is automatically assigned and based on the partition key hash it is put in a shard that it selects.</p>
<p style="text-align: justify;">KPL (Kinesis Producer Library) acts as an intermediary between producer and a stream, Kinda transactional in the sense that when asked to insert multiple records, if one of them fails insert due to an issue (eg throughput) it denies request. However, the application can still choose to insert a value regardless of other values failing insert.</p>
<p style="text-align: justify;">KCL (Kinesis Consumer Library) is used by consumers to read/consume values from the shards. Consumers can read from the beginning of the stream/latest/specific part of a stream based on the sequence number. The GetRecordsAPI requires an iterator.</p>
<p style="text-align: justify;">If the data is being read from the tip of the stream, start the consumers first and then the producers otherwise the consumers won't read beginning of the data.</p>
<p style="text-align: justify;">When storing data randomise partition keys to make sure of even distribution of data between shards. Provision few extra shards to handle unexpected demands. More shards are needed to support several consumers simultaneously reading from a stream.</p>
<p style="text-align: justify;">Batch data before writes for efficiency. On throughput exceeded error, retry/log and monitor/reshard shards. Increase DynamoDB table throughput in extreme cases. Best to have DynamoDB handle duplicates as two records with same keys will be an overwrite.</p>
<p style="text-align: justify;">Check permissions to make sure that the code has access to Amazon Kinesis streams, DynamoDB and CloudWatch services.</p>
<p style="text-align: justify;">Handle exceptions in processRecords method of IRecordProcessor object.</p>

<h2 style="text-align: justify;">Amazon Simple Work Flow (SWF)</h2>
<p style="text-align: justify;">Automate flow of events. Could also have long running human tasks. For instance, have a workflow to create and email an invoice and then wait for someone to click approve link within the email to then resume the workflow to further dispatch the product.</p>
<p style="text-align: justify;">Two types of work flow tasks: worker and decider. Decider can have code running behind it. Whatever thing the decider touches, it must have permission to access it. For instance, a decider to read from DynamoDB must have access to read data from DynamoDB. As the name suggests, the decider decides outcome. In the invoicing system, if the user clicks reject button in the invoice email, the decider can then cancel the order but continue with dispatch if the user clicks approve button.</p>
<p style="text-align: justify;">When the workflow starts the first time, SWF calls the decider which decides which step to go next. The control is then handed back to SWF from the decider and then based on the outcome of decider, SWF then goes to that step. The tasks that are executed can be executed asynchronously.</p>
<p style="text-align: justify;">No visual way to create the work flows out of the box. Requires programmatic creation via SDK.</p>

<h2 style="text-align: justify;">Amazon Simple Queue Service (SQS)</h2>
<p style="text-align: justify;">At least once delivery. Does not have all features of a fully fledged queuing solution. Highly scalable.</p>
<p style="text-align: justify;">Provides dead letter queues for messages that haven’t been delivered.</p>
<p style="text-align: justify;">Distributed nature means that some messages may not be available across all SQS servers.</p>
<p style="text-align: justify;">Message can appear in the queue after some time specified in delay seconds. Queue can restrict size of messages being put by specifying MaximumMessageSize. MessageRetentionPeriod specifies how long should a message be kept in the queue. This can be up to 14 days.</p>
<p style="text-align: justify;">While receiving messages, one can use short polling or long polling. Short polling means that SQS returns messages in short bursts (couple at a time), however, with Long polling, it blocks you until it finds significant number of messages. The block time is specified by the client, could be 5-10 seconds. Use this if processing large number of messages in batch is easier than in small batches.</p>
<p style="text-align: justify;">Visibility timeout is the time in which the message is invisible for a certain time and then becomes visible. This is for systems that need more time before they can pick up a message. The message handle can be used to extend this timeout if more time is needed.</p>
<p style="text-align: justify;">In Java, client side buffering is allowed where the messages can be pre fetched into a local buffer. Automatically batches messages for send or delete operations.</p>
<p style="text-align: justify;">Queues can be shared to another AWS accounts with or without credentials (anonymously). Admin can also create policies that grant access to a specific queue. This can then be assigned to a role.</p>
<p style="text-align: justify;">Requests incoming to a queue cannot be directly throttled. So if a queue is shared with someone else, they could potentially DDoS the queue and have you charged for each request which could be disastrous. However, this can be implicitly controlled by giving someone access to an Amazon API Gateway endpoint which then throttles all requests from there on to the SQS.</p>

<h2 style="text-align: justify;">Simple Notification Service (SNS)</h2>
<p style="text-align: justify;">Pub-sub model. Messages are sent to the SNS which then propagates them down to different services or applications. SNS messages are not persisted while SQS messages are. Also, interaction with SNS is passive (push) while the one with SQS is active (poll). Also, usually in SNS, one publisher talks to multiple consumers while SQS is usually meant for one publisher talking to one subscriber.</p>
<p style="text-align: justify;">Massively useful in decoupling applications. Supports event driven architecture where based on the message, the receivers could each do different things. For instance when an invoice is placed in SNS, one system that is subscribed could process the order associated with the invoice while second system could send email confirming the receipt of the order.</p>
<p style="text-align: justify;">SNS provides good fan out for massively parallel tasks. Example, lambda can listen for SNS messages, do the processing and then push to S3.</p>
<p style="text-align: justify;">Does not batch messages. Every notification is a message. Does not guarantee ordering of messages. SNS Delivery policy, that applies to HTTP subscribers can be used to control retries in case of message delivery failure. Messages can contain up to 256KB of data.</p>
<p style="text-align: justify;">Message sizes or format can be controlled using the message structure attribute of the message. For instance, in case of SMS message, the characters can be restricted to 160 characters. Refer to the documentation on the AWS website.</p>
<p style="text-align: justify;">Use cloud watch to keep track of stats about SNS.</p>

<h2 style="text-align: justify;">Lambda</h2>
<p style="text-align: justify;">Compute service similar to EC2. Automatically manages compute resources. Requires zero administration. Supports Python, Java (v8) and Node.js. Lets people focus on code, not administration. Removes need to have a lot of servers for a simple task. Hooks very well into Amazon AWS services. Known as connective tissue for AWS services. Eg. Fetch a file when it's uploaded to S3, do something with it and then send a SNS push notification with the resulting output of data.</p>
<p style="text-align: justify;">Once created, lambda function can be invoked by:</p>

<ol style="text-align: justify;">
	<li>Push model by publishing an event.</li>
	<li>Pull model by watching a stream or source (Kinesis stream or SQS queue polls)</li>
	<li>Synchronous invocation by calling a function or API directly from Lambda.</li>
</ol>
<h3 style="text-align: justify;">Push event model</h3>
<p style="text-align: justify;">Event source must itself invoke a lambda function directly. From Amazon Echo, S3, SNS or Cognito. Example: User pushes an item to S3. S3 then pushes an event to Lambda which then assumes an execution role to process the file and upload the processed file back to lambda.</p>

<h3 style="text-align: justify;">Pull model</h3>
<p style="text-align: justify;">Lambda polls the event service and then on event does things. For example polling a DynamoDB or Kinesis stream. The polling does not affect the cost associated with making requests. Upon event detection, it assumes Execution role and processes the event.</p>

<h3 style="text-align: justify;">Synchronous model</h3>
<p style="text-align: justify;">Invoke function in lambda using RequestResposne invocation type. Function executes and then returns immediately.</p>
<p style="text-align: justify;">When granting permissions, the source of the event must have permission to execute lambda functions. Whatever lambda then does must be able to execute operations that it is trying to do. For example, s3 upload event must have access to execute lambda. Then if the lambda function is uploading records to DynamoDB, it should have access to insert a DynamoDB record.</p>
<p style="text-align: justify;">If the role isn't specified, it assumes basic lambda role. The basic role has the least privilege of being able to write a log file.</p>
<p style="text-align: justify;">When writing a function, choose the amount of memory that you want to allocate. It then allocates CPU power. Additional memory can be configured in 64 MB increments upto 1536 MB. All calls are limited to five minutes of execution time. The default timeout is 3 seconds but can be set to any value between 1 and 300ms. Duration is calculated to nearest 100 milliseconds.</p>
<p style="text-align: justify;">Lambda functions be scheduled for execution. For example, get MI data every hour. Schedule can be set for fixed rate at every hour or 15 minutes. A crown expression can be provided.</p>
<p style="text-align: justify;">Packages for lambda functions can go up to 50 MB in size.</p>
<p style="text-align: justify;">Format for specifying handler for a Java application is &lt;package name&gt;.&lt;class name&gt;::&lt;function name&gt;</p>
<p style="text-align: justify;">Functions take longer to execute the first time because it creates a new container and allocates resources to it. However its faster from there on.</p>
<p style="text-align: justify;">Very easy integration with AWS API gateway where based on a request, a lambda function can be executed. The incoming request parameters can be mapped to lambda function parameters using a mapping template. For incoming JSON data, the mapping template allows mapping using json path.</p>
<p style="text-align: justify;">Sounds like TDD is the best practice for writing lambda functions. All the functions should be strictly tested before publishing them.</p>
<p style="text-align: justify;">When subsequent updates are pushed to a lambda function, it automatically versions them. It allows reverting back to an old version if the latest is broken. One can also specify a specific version to lambda to execute. One can also define aliases to lambda versions. Example, dev can point to latest version 5, system test points to 4 and production points to 3. Aliases are just pointers so if one wants to put the latest version in production then one only needs to change the production alias to latest version, eg. 5.</p>
<p style="text-align: justify;">Depending on what a lambda function is doing, it could be a cheaper alternative to an EC2 server.</p>
<p style="text-align: justify;">Lambda functions must be written in a stateless way with minimal overhead as they will be executed in a stateless manner. Default soft limit for executing lambda functions in parallel is 100. Depending on use, this could be extended by a simple request. Its advised to use versioning and aliases to deploy lambda functions.</p>
<p style="text-align: justify;">Dry run is a feature with Amazon Lambda that allows one to check permissions without actually executing the lambda function.</p>