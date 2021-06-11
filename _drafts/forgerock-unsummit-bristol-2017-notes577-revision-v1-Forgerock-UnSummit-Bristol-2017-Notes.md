---
id: 586
title: Forgerock UnSummit Bristol 2017 Notes
date: 2017-03-30T11:08:09+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2017/03/30/577-revision-v1/
permalink: /?p=586
---
## 10:00 Kickoff

[caption id="attachment_580" align="aligncenter" width="700"]<a href="https://www.manthanhd.com/wp-content/uploads/2017/03/IMG_1141.jpg"><img class="size-large wp-image-580" src="https://www.manthanhd.com/wp-content/uploads/2017/03/IMG_1141-700x525.jpg" alt="Agenda of the day" width="700" height="525" /></a> Finalised agenda of the day @ Forgerock UnSummit Bristol 2017[/caption]

## Forgerock on Docker

Start off with base docker image containing OpenAM. This docker image has a folder for config which can be volume mounted to allow loading custom config. During runtime, it detects the config and runs it against amster (ssoadm replacement).<!--more-->

Autonomous instances will allow clustering of OpenDJs so that the OpenAM servers don't have to know about individual OpenDJs.

`ssoadm` will for each command, create JVM, load context, run the command, destroy context and then finally destroy JVM. This slows down the tool by a significant amount. The new tool `amster` supports sessions where once fired, it creates a session and then any command can be run in that session more effectively than `ssoadm`.

All future versions of OpenAM will be able to import the configuration from their previous versions. So, OpenAM 15 will know out of the box how to import config from OpenAM 14.

New OpenDJ proxy allows orchestration of OpenDJ instances in a shard fashion. Helps with elastic scaling.

Docker expects logs to be in `stdout`. OpenAM has a new audit framework (common framework across all products) which can be used to send out logs to different output endpoints like `stdout`, `splunk` etc.

## Cloud Readiness

Use ELB / HA Proxy in front of OpenDJ instances within a region. This helps openam instances avoid knowing individual opendj servers that they have access to. Potentially use one ELB pe region, then use Route 53 to use geo-location to resolve to relavent ELB. Use replication manager to enable replication amongst those OpenDJ instances. General consensus on avoiding ELB as it has unpredictable behaviour at TCP level at times (struggles to handle big spikes of load since it needs to be prewarmed).

## Forgerock Roadmap

### OpenAM 14/14.5

OpenAM 14.5 brings stateless architecture to authentication modules. This means that the authentication chain can be used across openam servers without having to have sticky sessions.

Amster brings remote management to OpenAM. Unlike `ssoadm` where you had to be on the server in order to run it, `amster` can run remotely as it uses the openam's restful APIs.

Support for authentication trees (directed asyclic graphs) in order to enable consumers use complex flows.

(14.0) Push authentication allows authenticating mid authentication chain from mobile directly from the user. This can be done using something like Touch ID etc.

(14.5) Push authorisation allows authorising of specific transaction or activity mid session directly from the user. This can be done using something like Touch ID etc.

### OpenDJ 4/4.5

Proxy services will allow horizontal scalability. This is done by replicating the sharding architecture like the NoSQL databases.

- Loadbalancing and failover

- High availability

- Access control

Platform improvements will ease continuous delivery using tools like amster and being able to use kubernetes on top of docker containers. The release will come with official base docker containers which can be extended as needed.

- DevOps

- Docker containers

Database

- Memory Backend

Security

- Database encryption

## Stateless application architecture

Typically during authentication, when a token is issued, the token itself is a reference to its details that are stored in OpenDJ. This does not work at scale because of the eventual consistency with OpenDJ.

With stateless sessions, these details are stored in the token itself and when the token is presented, it is unpacked and validated (decrypted and signature verified).

However, this presents a new problem. What happens when the session is invalidated explicitly, outside of the session timeout? Well, this requires a black list. While the black list will only contain tokens that have been explicitly invalidated outside of their timeouts, once the timeout passes, they are removed. This keeps things tidy.

## OpenAM UI Customization

Setup themes in `ThemeConfiguration.js` file within the XUI war.