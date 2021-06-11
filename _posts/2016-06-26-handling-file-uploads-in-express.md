---
id: 380
title: Handling file uploads in Express using multer
date: 2016-06-26T01:33:26+01:00
author: Manthan Dave
layout: post
guid: https://www.manthanhd.com/?p=380
permalink: /2016/06/26/handling-file-uploads-in-express/
categories:
  - Findings
tags:
  - express
  - multer
  - node
  - npm
---
<p style="text-align: justify;">First of all, download and install <span class="lang:default decode:true crayon-inline">multer</span>.</p>

<pre class="lang:sh decode:true">npm install --save multer</pre>
<p style="text-align: justify;">Include the dependency.</p>

<pre class="lang:sh decode:true ">var multer = require("multer");</pre>
<p style="text-align: justify;">Initialise it into a service. The command below will initialise it with default storage type (being <span class="lang:default decode:true crayon-inline">DiskStorage</span>) and upload destination being the <span class="lang:default decode:true crayon-inline ">uploads/</span> folder.<!--more--></p>

<pre class="lang:sh decode:true ">var uploadService = multer({ dest: "uploads/" });</pre>
<p style="text-align: justify;">If you don't want the file to be saved anywhere in the file system and want to use memory storage, initialise it as follows.</p>

<pre class="lang:sh decode:true ">var uploadService = multer({ storage: multer.memoryStorage() });</pre>
<p style="text-align: justify;">You can add file size limits too. The command below sets the file upload limit to be 12 megabytes.</p>

<pre class="">var uploadService = multer({storage: multer.memoryStorage(), limits: {fileSize: 1000 * 1000 * 12}});</pre>
<p style="text-align: justify;">You can then pass around the <span class="lang:default decode:true crayon-inline">uploadService</span> object to other modules that use it.</p>

<pre class="">function FileUploadRoute(express, uploadService) {</pre>
<p style="text-align: justify;">To read uploads in a route, add it to the second parameter of the route definition. The following commands processes a single file uploaded under <span class="lang:sh decode:true crayon-inline ">file</span> attribute within post form-data.</p>

<pre class="">router.post("/", uploadService.single('file'), function (req, res) {
    var uploadedFile = req.file;
});</pre>
<p style="text-align: justify;">The <span class="lang:sh decode:true crayon-inline ">uploadedFile</span> object here contains data about the file that was uploaded. The <span class="lang:default decode:true crayon-inline">buffer</span> attribute of this object contains the actual file content. The object also has other attributes like <span class="lang:default decode:true crayon-inline">filename</span>, <span class="lang:default decode:true crayon-inline ">mimeType</span> and encoding that can be used to process the file according its type.</p>
<p style="text-align: justify;">For more information, see <a href="https://www.npmjs.com/package/multer" target="_blank">multer on npm</a>.</p>