---
layout: post
comments: true
author: islimani
title: Understanding OAuth2 
date: 2020-10-07 13:32:20 +0300
words: 1610
summary: OAuth2 is an open standard for token-based authentication and authorization on the Internet # Add post description (optional)
thumbnail: /assets/img/Google-OAuth.png
category:
        - OAuth2
---


## Introduction
If you’re a software developer on the web today, chances are you’ve heard of OAuth.
But what is it, how does it work, and why do we need it?


## What is OAuth 2.0?
*OAuth 2.0 is a delegation protocol, a means of letting someone who controls a resource allow a software application to access that resource on their behalf without impersonating them. It is used to protect a large (and growing) number of web APIs all over the world, from large-scale providers such as Facebook and Google to small startups.*[^1]

[^1]: OAuth2 in Action By RICHER and SANSO Manning Publications p3

Still unclear, think of this example.

**A mp3 player on the cloud, e.g soundcloud (client) wants to access your (the resource owner) mp3 files (protected resources) on Google Drive ( Google is the authorization server – see below)**

You don’t want to give the app access to (client) all your files on Google Drive, but only a limited access to mp3 files to enjoy your library. That’s where OAuth2 comes very handy.

It gives a limited access to an HTTP service on behalf of the resource owner to a third-party application without the need of the client to understand all the hassle of the protocol or the implemented security mechanism.
Have you ever seen  a web page or android app like the one in figure1, then you have used OAuth without even knowing about it!

![Figure 1 A Web App asking for authorization]({{site.baseurl}}/assets/img/oauth2.jpg)

## Components in OAuth 2.0
We have just mentioned all the components of OAuth 2.0 in our example. You see, it’s just that simple.
These are the components that we have so far:

* *The resource owner has access to an API and can delegate access to that API. The resource owner is usually a person and is generally assumed to have access to a web browser.[^2]  (The user with the Google Account in the last example)*

* *The protected resource is the component that the resource owner has access to. This can take many different forms, but for the most part it’s a web API of some kind.[^3]  (mp3 files)*

* *The client is the piece of software that accesses the protected resource on behalf of the resource owner.[^4]  (web app, e.g soundcloud)*

* *The authorization server (AS) is trusted by the protected resource to issue special purpose
security credentials—called OAuth access tokens—to clients.[^5]  (Google).*

[^2]: OAuth2 in Action By RICHER and SANSO Manning Publications p5
[^3]: OAuth2 in Action By RICHER and SANSO Manning Publications p6
[^4]: OAuth2 in Action By RICHER and SANSO Manning Publications p6
[^5]: OAuth2 in Action By RICHER and SANSO Manning Publications p11

## So How does OAuth 2 work?

* Simply put, the client first sends the resource owner to the authorization server in order to authorize the client.

* The resource owner authenticates to the authorization server and is generally presented with a choice of whether to authorize the client making the request (figure1).

* Once the authorization grant has been made, the client can then request an access token from the authorization server.

* This access token can be used at the protected resource to access the API, as granted by the resource owner.

## What now?

In the next part, we will be looking into real examples using Spring Framework.

Stay Tuned!




