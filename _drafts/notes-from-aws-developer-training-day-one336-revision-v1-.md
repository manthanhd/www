---
id: 338
date: 2016-03-02T22:16:53+00:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2016/03/02/336-revision-v1/
permalink: /?p=338
---
<h2 style="text-align: justify;">Basics</h2>
<p style="text-align: justify;">Service client API has objects for request and response data. Contrasting old way of retrieving things using the AWS SDK with the new way, it looks like they have switched from SOAP API to a RESTful API. The old way requires you to create Request and Response objects every time you want to do anything with the AWS API. Looking into the implementation, this looks a lot like SOAP.</p>
<p style="text-align: justify;">The new way is a lot more neater. A request can be built using builders that work on a conceptual model of the request. Response is also a lot more conceptual and easy to read.</p>

<h3 style="text-align: justify;">Signing requests with your credentials</h3>
<p style="text-align: justify;">Signature prevents the request from being tampered as the signature will become invalid as soon as someone changes the request in transit. It is also time based so that the replay attacks can be prevented.</p>
<p style="text-align: justify;">SDK signs requests automatically so normally no manual intervention is required.</p>

<h2 style="text-align: justify;">SDKs</h2>
<p style="text-align: justify;">Regarding regions, when instantiating the service client, a region must be set. In all languages, except Java, if a region isn't specified, it will go and find the region specified in .aws/credentials file under [default].</p>
<p style="text-align: justify;">Exceptions are of two types in Java and .Net: AmazonServiceException is exception coming from the service itself, I.e. Remote. AmazonClientException is an exception happening on the client side due to bad request. This is slightly different in JavaScript due to callbacks. Here, callbacks are always in function(error, data) format.</p>
<p style="text-align: justify;">SDK will retry most requests that fail except ones that have service errors like access denied etc.</p>
<p style="text-align: justify;">A set of client credentials won't be locked out unless all the requests that are coming in look like an attack.</p>

<h2 style="text-align: justify;">AWS Data Storage</h2>
<p style="text-align: justify;">S3, Glacier, DynamoDB, ElastiCache, RDS, RedShift.</p>
<p style="text-align: justify;">Every byte you change will need a reupload of the whole file. Not good for data that is changing constantly. Use S3 if you want your data to be read a lot more than changed. Highly scalable and durable. Allows hosting of static websites. Two plans: S3 Standard and S3 Standard - Infrequent Access.</p>
<p style="text-align: justify;">Use Glacier for files that you want to keep for long time. Archival solution, cheap long term storage. Retrieval is slow, about 3-5 hours to process a file retrieval. Old backups, auditing data, log files for keep sake should go in Glacier. Create a job in S3 that moves a file that hasn't been read in one or two months to Glacier and then another policy that deletes old data after 3-5 years from Glacier.</p>
<p style="text-align: justify;">DynamoDB NoSQL Key Value pair solution. For data that's not too complex or relational or transactional. Scales horizontally as per need.</p>
<p style="text-align: justify;">RDS is traditional relational database. Use for transactional or relational data. Scales horizontally with automatic failover to secondary instances. Allows variety of engines like Amazon Aurora, Oracle, MS SQL Server, Postgres, MySQL etc.</p>
<p style="text-align: justify;">ElastiCache caching solution. Store lightweight short lived data. Blazing fast retrieval. Allows creation and scale cache cluster. Based on memcache and redis.</p>
<p style="text-align: justify;">RedShift allows complex queries on massive data sets (petabyte scale). Data warehousing solution. Massively parallel model. Takes your query, compiles it and runs it in parallel across a large number of instances. Customers could take their load balancer logs, put them into RedShift and then compile list of customers and their frequency including their country of origin. RedShift provides JDBC and ODBC drivers to enable you to use familiar SQL client tools.</p>

<h2 style="text-align: justify;">Developing storage solutions with Amazon S3</h2>
<p style="text-align: justify;">Data is stored in S3 buckets as objects. The bucket names must be globally unique. Bucket names: 3-63 chars, lowercase letters, numbers, hyphens and no periods.</p>
<p style="text-align: justify;">When storing files/objects in a bucket, prefer to use the path structure for file names so that rules can be created easily. Paths actually has no meaning to S3 but it's useful for us as it enables rule creation.</p>
<p style="text-align: justify;">For retrieval, use path style https://&lt;region specific endpoint&gt;/&lt;bucket name&gt;/&lt;object name&gt; or virtual hosted style http://&lt;bucket name&gt;.s3.amazonaws.com/&lt;object name&gt;.</p>
<p style="text-align: justify;">While uploading files, if the file size is &gt; 100MB, use multi-part upload. Usually this rule is for files that are &gt; 5GB.</p>
<p style="text-align: justify;">While reading, you can read the entire file or only read a specific range of bytes.</p>
<p style="text-align: justify;">While deleting, if versioning is enabled, on delete it will revert to the previous version if it exists. However, if versioning is not enabled then the file is permanently deleted. For objects or buckets backed up to glacier, they can be restored back at any time.</p>
<p style="text-align: justify;">Pre-signed URLs allows one to provide access to someone to upload or download a file to bucket to/from S3. Pre-signed URLs expire after a specific time period. This period by default is ….</p>
<p style="text-align: justify;">SSL-encrypted end points allow security of data in transit.</p>
<p style="text-align: justify;">S3 allows CORS to be specified. Add domains to it so that browser requests are allowed. Ex. Allow <a href="http://www.capitalone.com">www.capitalone.com</a> to access fonts bucket.</p>
<p style="text-align: justify;">For high performance, randomise part at the beginning of the key. This is because of the way S3 hashes URLs. If the beginning part is the same, all files end up on the same partition which will create bottleneck.</p>
<p style="text-align: justify;">Avoid unnecessary requests. S3 doesn't return back 304 for subsequent requests, it will always serve the file. Use a CDN instead.</p>
<p style="text-align: justify;">Verbose logging is allowed for S3. Use this only in development, and not on production system. Every HTTP request generates a request ID pair in logs. Reference this request ID when contacting AWS support.</p>

<h2 style="text-align: justify;">Storing data using DynamoDB</h2>
<p style="text-align: justify;">Can store JSON documents but these must be looked up by the key only.</p>
<p style="text-align: justify;">Common use cases: Gaming, AdTech, Mobile apps, IoT. Send small data with very high frequency eg. Analytics. Record size must be less than 400KB. Each table has a key attribute which is the partition key. The partition key is hashed and then the data is put in that partition. Hence similar data must have same/similar partition key.</p>
<p style="text-align: justify;">Sort key is optional. For example, date of the transaction can be the sort key. This is the key that can be used for secondary lookup. For instance, if partition key is A and sort key is B, when doing lookup by A, it will return all data matching partition key A. However, we can narrow the search by looking up by partition key A and sort key B.</p>
<p style="text-align: justify;">DynamoDB is eventually consistent, usually within a second of the operation. Request can specify strong consistency but that means data might take longer to write and will cost more. Default is eventual consistency.</p>
<p style="text-align: justify;">Read capacity is reads per second of items up to 4 KB in size. Write capacity unit is in number of 1 KB writes per second.</p>
<p style="text-align: justify;">Important to choose the right partition key as it directly affects the throughput. The total throughput is divided across partitions so if a set of partition keys are being looked up more often than others Then it means that the partition key must be fixed or changed.</p>
<p style="text-align: justify;">The secondary index can be changed from the default.</p>
<p style="text-align: justify;">Streams allow firing of events based on an event. For instance, if user Jane changes her exercise from Walking to Spinning, then an update event is fired to all other systems notifying them of the change. Works much like Kinesis streams.</p>
<p style="text-align: justify;">Values can be retrieved matching a specific primary key condition. THis can be refined by providing a filter expression. If the data is looked up by a field that is not index, a scan operation will happen which is much slower than index lookup as it has to scan through the entire database looking for the record.</p>
<p style="text-align: justify;">Pagination: returns up to 1 MB of data or limit by number of items to be returned, like 2 records.</p>
<p style="text-align: justify;">Query can return many items, getItem returns a specific item by key. PutItem adds a new item. UpdateItem updates a specific item. RemoveItem means removing a specific attribute from an item while DeleteItem deletes a whole item. AddItem adds an attribute to an item.</p>
<p style="text-align: justify;">Writes provide free reads as the written value is returned when it's written. Write operations can be made conditional. For instance perform write operation only when account lock timeout has passed.</p>
<p style="text-align: justify;">In Java and .NET there's a persistence model available. This allows the client to do client side operations.</p>
<p style="text-align: justify;">DynamoDB local is a client side application that mimics DynamoDB service. Can be used for testing applications before production and avoid additional cost.</p>
<p style="text-align: justify;">Also comes with access management to control access to resources and actions.</p>

<h2 style="text-align: justify;">Best practice</h2>
<p style="text-align: justify;">Make sure that there are no hot (overused) partitions. Caching can help relieve this but ideally design partition keys so that this doesn't happen in first place.</p>
<p style="text-align: justify;">Separate data that is not used very frequently on a different partitions and then adjust throughout. For instance, latest data can go on a high throughput partitions and old data is moved to one of the older partitions with lower throughput.</p>
<p style="text-align: justify;">Use one to many tables instead of one table with large number of attributes. This is more scalable. Store frequently accessed small attributes in a separate table. Example separate company stock table from company details as the stock table will have information updated and read more frequently than details.</p>
<p style="text-align: justify;">Max size for value is 400KB, can store binary.</p>