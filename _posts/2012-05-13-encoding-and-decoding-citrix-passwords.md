---
id: 2586
title: 'Encoding and Decoding Citrix Passwords'
date: '2012-05-13T21:55:02+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2586'
permalink: /2012/05/13/encoding-and-decoding-citrix-passwords/
short-url:
    - 'http://bit.ly/IORGJx'
views:
    - '10310'
categories:
    - Citrix
tags:
    - Citrix
    - Password
    - 'Web Interface'
---

I am working on a launcher tool for Citrix XenApp that can not only connect to a published application or published desktop but can also leverage Citrix Workspace Control to reconnect to disconnected and/or active sessions.

There doesn’t seem to be any sdk that exposed the data we need so I am trying to reproduce what the Citrix online plugi-in does.

I used a HTTP monitoring tool to capture the traffic between the Online plug-in and the Web Interface. First the online plug-in will retrieve the config.xml from the server specified via the Change Server option:

[![What is the address of the server hosting your published resources? | Server Address | Example: servername (for non-secure connections) | https://servername (for secure connections)](http://192.168.40.25:8081/wp-content/uploads/2012/05/image_thumb2.png "Change Server - Citrix online plug-in")](http://192.168.40.25:8081/wp-content/uploads/2012/05/image2.png)

The config.xml is a rather large xml file, the interesting part is the Request.Enumeration (I left out the other data):

“&gt;

From this xml data, the enum.aspx url is taken and another HTTP post is sent to that url which contains the following xml in my case:

“&gt;

Notice that the password is encoded so if we want to replicate the HTTP post data we need to be able to encode (and perhaps decode) the password.

The decoding seems to be named Ctx1 but I couldn’t find any information on how it should be encoded so I had to find it out myself.

I wrote a tool that that can encode and decode the passwords and I suspect the password decoding is the same as used for storing passwords in ica files (I haven’t checked that yet…):

[![Encrypt | Decrypt Password | Hash | Citrix | Ctx1](http://192.168.40.25:8081/wp-content/uploads/2012/05/image_thumb3.png "Citrix Password Hasher by Remko Weijnen")](http://192.168.40.25:8081/wp-content/uploads/2012/05/image3.png)

The tool can be downloaded below.

[ Citrix Password Encoding &amp; Decoding Utility (4926 downloads ) ](http://192.168.40.25:8081/download/citrix-password-encoding-decoding-utility/?tmstv=1726048919 "Version 1.0")