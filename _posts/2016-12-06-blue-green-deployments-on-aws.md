---
id: 517
title: Blue-green deployments on AWS
date: 2016-12-06T10:53:47+00:00
author: Manthan Dave
layout: post
guid: https://www.manthanhd.com/?p=517
permalink: /2016/12/06/blue-green-deployments-on-aws/
image: /wp-content/uploads/2016/12/blue-green-banner.png
categories:
  - Educational
  - Findings
tags:
  - aws
  - cloud
  - cloud formation
  - deployment
  - java
---
<p style="text-align: justify;">Before we do this, make sure that you have a service that you want to deploy. To keep things simple, I followed the spring boot tutorial on making a restful web service. It was quick and the app worked like a charm. As usual, I went a bit extra and made my app return a stubbed list of users. You don't have to. Make sure you have a <span class="lang:default decode:true crayon-inline">/healthcheck</span> endpoint and another endpoint that you can test with. In my case, I have <span class="lang:default decode:true crayon-inline ">/users</span> which returns a list of users.</p>
<p style="text-align: justify;">All righty then. Lets get a high level overview of what things are and how they are going to work. But before we do that, lets go through a quick real-ish life scenario.</p>
<p style="text-align: justify;">Say you have a service that you have deployed onto AWS. Now you have a newer version of that service that you'd like to test. Since you never know if something works without actually trying it out, normally, after exhaustive testing in staging and other environments, you'd deploy that service into production to all your users. But ah ha! That one guy in your team forgot that one test case which made it blow up which means every single user of yours is now seeing error pages everywhere. This is bad so you roll it back to the previous version. Doesn't sound too bad yet but by the time you do this, you'd have lost a couple of hours in time which would translate into actual money lost to the company which could eventually make a dent in your end of the year bonus.</p>
<p style="text-align: justify;"><!--more--></p>
<p style="text-align: justify;">But what if there was a better way. What if you could've deployed that change to only one of your hundred servers. This would mean that if it does blow up, it would've only affected one percent of your users (or rather more accurately one percent of servers). This better way is the blue-green deployments.</p>
<p style="text-align: justify;">Blue-green deployments (a.k.a A/B deployments) are used quite often as part of software development lifecycle to test out a certain piece of functionality in a live production environment for a select group of users. Basically what I said but in a fancier language.</p>
<p style="text-align: justify;"><a href="https://www.manthanhd.com/wp-content/uploads/2016/12/blue-green-deployments.png"><img class="aligncenter size-full wp-image-518" src="https://www.manthanhd.com/wp-content/uploads/2016/12/blue-green-deployments.png" alt="blue green deployments image representation" width="349" height="328" /></a></p>
<p style="text-align: justify;">In order to do this, we need to break down the AWS infrastructure our service depends on. In most cases, the following resources are used:</p>

<ul style="text-align: justify;">
 	<li>Elastic Load Balancing (ELB)</li>
 	<li>Autoscaling Groups (ASG)</li>
 	<li>Launch Configuration</li>
</ul>
<p style="text-align: justify;">Note that I have not explicitly called out Elastic Compute (EC2) instances in the above list of resources. This is mainly because we are implicitly using them, via the launch configuration but are not explicitly creating a resource of that type. Also, at this level, blue-green does not have anything to do with databases which is why we'll leave it out this time.</p>
<p style="text-align: justify;">Like most people, you would have a single cloud formation script with all the above resources in it. Whilst this is great, to do blue-green deployments, we'll need to break it down. You could go as fine as one cloud formation script per resource, however, I have them down into two scripts; first for ELB and second for ASG as well as launch configuration. I tried breaking them down into three scripts but something about that didn't quite feel right as I felt that ASG and launch configuration are quite tightly coupled as is.</p>
<p style="text-align: justify;">The way this is going to work is that we'll deploy an ELB using the ELB cloud formation scripts across two availability zones. In my case, this is eu-west-1a and eu-west-1b. This ELB will have a unique LoadBalancerName attribute. In our case, since we are only doing single region, this will be  something like UserServiceLB.</p>
<p style="text-align: justify;">Once that is deployed, we will then deploy the blue service stack using the second cloud formation script which will deploy our launch configuration as well as a auto scaling group. This auto scaling group will need to be attached to a load balancer. Yep, you guessed it. We'll specify the previously defined unique LoadBalancerName here. So, say for instance UserServiceV1 (blue), this will look like following:</p>
<p style="text-align: justify;"><a href="https://www.manthanhd.com/wp-content/uploads/2016/12/cf-deployment.png"><img class="aligncenter size-full wp-image-519" src="https://www.manthanhd.com/wp-content/uploads/2016/12/cf-deployment.png" alt="aws blue green deployment image representation" width="509" height="319" /></a></p>
<p style="text-align: justify;">If, say now we want to deploy V2 (green) of the service, we'll have to deploy another stack with different launch configuration but still have the autoscaling group pointing at the same ELB. This might look like following:</p>
<p style="text-align: justify;"><a href="https://www.manthanhd.com/wp-content/uploads/2016/12/cf-deployment-2.png"><img class="aligncenter size-large wp-image-521" src="https://www.manthanhd.com/wp-content/uploads/2016/12/cf-deployment-2-700x294.png" alt="cf-deployment-2" width="700" height="294" /></a></p>
<p style="text-align: justify;">Now the ELB will send some of the traffic to the new green (color not indicative of health) instances. Because this is only some of the traffic, if something does go wrong, it is only impacted to a small subset of customers. Also, if it all goes horribly wrong (murphy's law not intended), we can simply delete the green stack and it will all happily keep ticking.</p>
<p style="text-align: justify;">If the new green stack is proving its worth, we can make changes to the autoscaling group to increase the number of instances while at the same time making changes to the blue stack to decrease the number of its instances. As you can see, all this can quite easily be automated.</p>
<p style="text-align: justify;">Now lets get on with the how. These are the scripts that I used.</p>
<p style="text-align: justify;">elb.json:</p>

<pre class="lang:js decode:true" title="elb.json">{
  "Parameters": {
    "LoadBalancerName": {
      "Type": "String",
      "Description": "Name of this loadbalancer. This is the name that you must specify when attaching an autoscaling group."
    }
  },
  "Resources": {
    "UserServiceELB": {
      "Type" : "AWS::ElasticLoadBalancing::LoadBalancer",
      "Properties": {
        "LoadBalancerName": {"Ref": "LoadBalancerName"},
        "SecurityGroups": ["sg-xxxxxxx"],
        "AvailabilityZones": ["eu-west-1a", "eu-west-1b"],
        "Listeners" : [ {
          "LoadBalancerPort" : "80",
          "InstancePort" : "8080",
          "Protocol" : "HTTP"
        } ],
        "HealthCheck" : {
          "Target" : "HTTP:8080/healthcheck",
          "HealthyThreshold" : "3",
          "UnhealthyThreshold" : "5",
          "Interval" : "30",
          "Timeout" : "5"
        },
        "Tags": [
          {"Key": "Name", "Value": {"Ref": "LoadBalancerName"}}
        ]
      }
    }
  }
}</pre>
<p style="text-align: justify;">This is the one that actually deploys the load balancer. Make sure that you replace the security groups with one of yours or else the script won't work.</p>
<p style="text-align: justify;">So lets quickly examine what this script is doing. This one deploys a single resource of type LoadBalancer. This is a AWS Classic Load balancer. Why not V2? Well, this is what we need for now. I think we might need V2 when we do containerised deployments. I'll cover that some another time.</p>
<p style="text-align: justify;">Listeners section indicates that the load balancer is listening on port 80 while forwarding requests to port 8080 on instances. If your sample app is listening on a different port, other than 8080, you will have to change the value for InstancePort attribute.</p>
<p style="text-align: justify;">The HealthCheck section lets the loadbalancer know what to health check on. In this case, it is calling /healthcheck endpoint on my app. This will let it know whether or not the instance is listening correctly. The URL doesn't need to be /healthcheck, it can be anything as long as it returns 200.</p>
<p style="text-align: justify;">If you upload this cloud formation to the AWS web console, you will be prompted to enter a value for the LoadBalancerName parameter (as defined in the Parameters section). Whatever value you enter, make a note of it.</p>
<p style="text-align: justify;">Next, we'll deploy the app itself.</p>
<p style="text-align: justify;">app.json:</p>

<pre class="lang:js decode:true" title="app.json">{
  "Parameters": {
    "ASGMinSize": {
      "Type": "Number",
      "Default": "1",
      "Description": "Minimum number of instances required for this app's autoscaling group"
    },
    "ASGMaxSize": {
      "Type": "Number",
      "Default": "1",
      "Description": "Maximum number of instances required for this app's autoscaling group"
    },
    "LoadBalancerName": {
      "Type": "String",
      "Description": "Name of the loadbalancer to attach this app's autoscaling group to"
    }
  },
  "Resources": {
    "UserServiceScalingGroup": {
      "Type" : "AWS::AutoScaling::AutoScalingGroup",
      "Properties" : {
        "AvailabilityZones": ["eu-west-1a", "eu-west-1b"],
        "LaunchConfigurationName" : { "Ref" : "UserServiceLaunchConfig" },
        "MinSize" : {"Ref": "ASGMinSize"},
        "MaxSize" : {"Ref": "ASGMaxSize"},
        "LoadBalancerNames" : [ {"Ref": "LoadBalancerName"} ]
      }
    },
    "UserServiceLaunchConfig": {
      "Type" : "AWS::AutoScaling::LaunchConfiguration",
      "Properties": {
        "KeyName" : "aws-keypair",
        "ImageId": "ami-9398d3e0",
        "SecurityGroups": ["sg-xxxxxxx"],
        "InstanceType": "t2.nano",
        "IamInstanceProfile": "arn:aws:iam::999987541517:instance-profile/UserServiceRoles-UserServiceInstanceProfile-4D8TE56SDVZM",
        "UserData"       : { "Fn::Base64" : { "Fn::Join" : ["", [
          "#!/bin/bash -xe\n",
          "yum update -y\n",
          "yum remove -y java-1.7.0-openjdk\n",
          "yum install -y java-1.8.0-openjdk-headless.x86_64\n",
          "aws s3 cp s3://manthanhd-buildartifacts/services/userservice/1.0.0/user-service-1.0.0.jar .\n",
          "/bin/env java -jar user-service-1.0.0.jar\n"
        ]]}}
      }
    }
  }
}</pre>
<p style="text-align: justify;">This will deploy the launch configuration as well as the autoscaling group. Again, make sure you replace the SecurityGroups attribute with value of your own or else the script won't work. Also, note that the UserData is configured to deploy a java application because, as I mentioned in the beginning, I am deploying a Spring Boot service. If you are using something else other than a java based service, you might have to change it to suit your needs.</p>
<p style="text-align: justify;">Let's quickly go through that cloud formation script. As you can see, we are deploying one autoscaling group and one launch configuration. The auto scaling group defines the min size, max size, name of the load balancer as well as the name of the launch configuration. The first three parameters are referenced from the parameters section whereas the launch configuration is a live reference from the launch configuration resource below. Also, note that we are deploying the autoscaling group into two availability zones within eu-west region, same as the load balancer.</p>
<p style="text-align: justify;">The launch configuration is rather simple. It basically instucts AWS as to how to launch the instance. The imageId attribute is the default AWS AMI. I've made the KeyName generic as in aws-keypair. It doesn't need to have a value, however, it is useful when you need to debug something.</p>
<p style="text-align: justify;">The launch configuration also attaches instance profile to the launched EC2 instance. In my case, this is a restricted instance profile that only allows the EC2 instance to get and list objects in a specific folder within my S3 bucket.</p>
<p style="text-align: justify;">If you refer back to the architecture diagram, you'll notice that we are deploying the launch configuration and autoscaling group separately to the load balancer. During the deployment of the autoscaling group, we are specifying the load balancer to attach the autoscaling group to. This is where you need to use the LoadBalancerName attribute that you previously noted down.</p>
<p style="text-align: justify;">Once you have roles and security groups in place with updated scripts, go ahead and deploy the elb.json as well as app.json. For app.json, make sure you suffix the stack name with V1 so that we know that this is the first deployment.</p>
<p style="text-align: justify;">This will create three instances under the elb. Now deploy the app.json again but this time with only one instance. Lets pretend that this is a new version. Make sure you suffix the stack name with V2. Once deployed, you will see that your new instance has been attached to the same ELB. Really cool right? Now, you could just update that existing V1 stack. Go back to app.json and change the instance count to 2 and update the V1 stack. Now you should see AWS shrink that autoscaling group to 2 from 3 whilst V2 is still 1. You get the idea.</p>
<p style="text-align: justify;">All this, can of course be quite easily automated using the AWS's CLI and APIs.</p>
<p style="text-align: justify;">Hope this has helped drive your blue green deployments on AWS. I have been piecing together notes for a while and finally managed to drive a PoC out. You can find the code here:</p>
<p style="text-align: justify;"><a href="https://github.com/manthanhd/aws-blue-green-java">https://github.com/manthanhd/aws-blue-green-java</a></p>