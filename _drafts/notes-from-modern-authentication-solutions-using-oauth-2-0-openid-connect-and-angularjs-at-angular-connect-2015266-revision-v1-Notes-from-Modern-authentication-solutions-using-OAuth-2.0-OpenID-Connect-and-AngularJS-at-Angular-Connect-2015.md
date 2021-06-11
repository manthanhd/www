---
id: 267
title: Notes from Modern authentication solutions using OAuth 2.0, OpenID Connect and AngularJS at Angular Connect 2015
date: 2015-10-22T12:02:27+01:00
author: Manthan Dave
layout: revision
guid: https://www.manthanhd.com/2015/10/22/266-revision-v1/
permalink: /?p=267
---
Day two 

0945

Modern authentication solutions with oauth 2.0, openid connect and angularjs by Manfred Steyer 

One client has too many accounts. One application is bound to many other applications. Lack of trust between applications. 

Oauth was developed at twitter and ma.golia

Standard for delegation of restricted rights. 

<!--more-->

Client wants to access a resource server on the behalf of the user who owns the resource. The authorization server has access to the resource. 

Client sends the scope to the authorization server. The auth server then asks the user if the client should be granted permissions. The user then agrees if he wants to and then the auth server then sends the access token. Which the client then uses to get access to the restricted resources. 

Advantage is the client doesn't have the password. The authorization server helps segregate duties and concerns between parties. 

Flows

Authorisation code grant is designed for server side applications. 

Implicit grant is designed for single page applications. 

Now talking about Implicit grant. 

Openid connect defines how to use Oauth for authentication. Client gets id token which is a jwt token with information about the user. Can be signed by the issuer. 

Id token is for the client while the access token is for the resource server. 

Now showing the demo. 

It's a single page application that provides some vouchers. To buy the voucher the user has to be logged in. 

Showing the oauth network flow using fiddler.