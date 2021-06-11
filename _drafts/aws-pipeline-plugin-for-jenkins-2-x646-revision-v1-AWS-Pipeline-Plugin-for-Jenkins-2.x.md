---
id: 649
title: AWS Pipeline Plugin for Jenkins 2.x
date: 2017-08-14T14:15:43+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2017/08/14/646-revision-v1/
permalink: /?p=649
---
I found this really cool plugin last week so I thought I'd make a post out of it.

I heavily use the "AssumeRole" capability within my application. Previously, this is what I used:
<pre class="lang:sh decode:true">/usr/bin/env aws sts assume-role --region ${AWS_DEFAULT_REGION} --role-arn ${CROSS_ACCOUNT_ROLE} --role-session-name "AssumedSession" &gt; /tmp/tmp.txt

export AWS_ACCESS_KEY_ID=$(grep -Po '(?&lt;="AccessKeyId": ")[^"]*' /tmp/session.txt | tr -d '\n')
export AWS_SECRET_ACCESS_KEY=$(grep -Po '(?&lt;="SecretAccessKey": ")[^"]*' /tmp/session.txt | tr -d '\n')
export AWS_SESSION_TOKEN=$(grep -Po '(?&lt;="SessionToken": ")[^"]*' /tmp/session.txt | tr -d '\n')
export TOKEN_ISSUED_SECONDS=${SECONDS}

rm /tmp/session.txt</pre>
As you can see, the code above is using shell commands to achieve the assume role awesomeness. This works well when your jenkins job has a shell command step but not when you have a groovy pipeline defined in your Jenkins.

I struggled with it initially - one of the ideas I had was to upload the assume script somewhere like S3 and then pull it down and run it when I wanted to run commands under the assume-role. However, this felt a bit cumbersome.

Next thing in my mind was to write a groovy plugin that can do this for me. However, rather than reinventing the wheel, I started looking for existing solutions. Finally, I found the <a href="https://wiki.jenkins.io/display/JENKINS/Pipeline+AWS+Plugin">aws-pipeline-plugin</a>.

Its a neat little plugin that allows you to do a bunch of basic stuff that you might want to do on AWS. Assume role is one of them.

So now with the new plugin, my code reduced to:
<pre class="lang:default decode:true ">withAWS(role: roleToAssume, roleAccount: myAwsAccountId) {
    sh "aws s3 cp file.txt s3://${bucketName}${uploadPath}"
}</pre>
Here, I've got a couple of variables but their names should be self-explanatory of their purpose. The general idea is that anything you write within that <code>withAWS</code> block will get executed under the role specified in <code>role</code> variable.