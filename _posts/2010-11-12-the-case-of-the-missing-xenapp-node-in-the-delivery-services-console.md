---
id: 759
title: 'The case of the missing XenApp Node in the Delivery Services Console'
date: '2010-11-12T12:37:44+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=759'
permalink: /2010/11/12/the-case-of-the-missing-xenapp-node-in-the-delivery-services-console/
views:
    - '6362'
short-url:
    - 'http://bit.ly/glQVai'
categories:
    - Citrix
---

After having successfully tested the Unattended install jobs for my Citrix XenApp 5 environment I went on to testing the install.

The install itsself and the msi logs that my jobs created all indicated that the install was successfull.

So I went on and launched the install jop for the Delivery Services Console. This job succeeded nicely but when I opened the console the XenApp node was missing:

[![XenApp](http://192.168.40.25:8081/wp-content/uploads/2010/11/xenapp-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/11/xenapp.png)

The resolution is described in [CTX125827](http://support.citrix.com/article/CTX125827 "Presentation Server or XenApp Nodes are Missing in the Access Management Console or Delivery Services Console") but Resolution 2 is a lot of manual labour 🙁

So what do we do? We script it!  
“&gt;