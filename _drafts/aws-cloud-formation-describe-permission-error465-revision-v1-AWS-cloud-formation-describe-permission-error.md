---
id: 491
title: 'AWS cloud formation describe* permission error'
date: 2016-10-16T19:38:04+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2016/10/16/465-revision-v1/
permalink: /?p=491
---
So you know sometimes, its difficult to work with AWS cloud formation scripts. On surface, errors are seemingly random and unrelated to the script. This is what happened today. I wrote an awesome parameterised cloud formation script. I was quite proud of it, mainly because most of my parameters were typed. This mean that the parameters like, ec2_security_groups had a type of <span class="lang:default decode:true crayon-inline">List&lt;AWS::EC2::SecurityGroup::Id&gt;</span>. This not only makes it easier to work with that cloud formation script from the AWS console, but also makes it very easy to work with from a CI/CD pipeline as if those parameters are invalid, the script will fail instantly, instead of waiting for resources to deploy.

However, while doing this, I completely missed the fact that in order for cloud formation to validate your input parameters, it needs to look them up first. What IAM policy permission does a resource lookup need? <span class="lang:default decode:true crayon-inline">describe</span>!

For once, my IAM role had tightly restricted permissions and because of this I had to go on to expand them slightly to allow for describe permissions.

I kept getting this error about the role not having the describe permission and I kept wondering why it needed that. Well now you know too!