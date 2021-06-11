---
id: 381
title: Handling file uploads in Express
date: 2016-06-26T01:30:52+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2016/06/26/380-revision-v1/
permalink: /?p=381
---
First of all, download and install <span class="lang:default decode:true  crayon-inline">multer</span>.
<pre class="lang:sh decode:true">npm install --save multer</pre>
Include the dependency.
<pre class="lang:sh decode:true ">var multer = require("multer");</pre>
Initialise it into a service. The command below will initialise it with default storage type (being <span class="lang:default decode:true  crayon-inline">DiskStorage</span>) and upload destination being the <span class="lang:default decode:true  crayon-inline ">uploads/</span> folder.
<pre class="lang:sh decode:true ">var uploadService = multer({ dest: "uploads/" });</pre>
If you don't want the file to be saved anywhere in the file system and want to use memory storage, initialise it as follows.
<pre class="lang:sh decode:true ">var uploadService = multer({ storage: multer.memoryStorage() });</pre>
You can add file size limits too. The command below sets the file upload limit to be 12 megabytes.
<pre class="">var uploadService = multer({storage: multer.memoryStorage(), limits: {fileSize: 1000 * 1000 * 12}});</pre>
You can then pass around the <span class="lang:default decode:true  crayon-inline">uploadService</span> object to other modules that use it.
<pre class="">function FileUploadRoute(express, <strong>uploadService</strong>) {</pre>
To read uploads in a route, add it to the second parameter of the route definition. The following commands processes a single file uploaded under <span class="lang:sh decode:true  crayon-inline ">file</span> attribute within post form-data.
<pre class="">router.post("/", uploadService.single('file'), function (req, res) {
    var uploadedFile = req.file;
});</pre>
The <span class="lang:sh decode:true  crayon-inline ">uploadedFile</span> object here contains data about the file that was uploaded. The <span class="lang:default decode:true  crayon-inline">buffer</span> attribute of this object contains the actual file content. The object also has other attributes like <span class="lang:default decode:true  crayon-inline">filename</span>, <span class="lang:default decode:true  crayon-inline ">mimeType</span> and encoding that can be used to process the file according its type.

For more information, see <a href="https://www.npmjs.com/package/multer" target="_blank">multer on npm</a>.