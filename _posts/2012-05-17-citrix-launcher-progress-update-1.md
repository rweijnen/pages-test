---
id: 2609
title: 'Citrix Launcher Progress Update 1'
date: '2012-05-17T23:19:12+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2609'
permalink: /2012/05/17/citrix-launcher-progress-update-1/
short-url:
    - 'http://bit.ly/JP5Yef'
views:
    - '1862'
categories:
    - Citrix
    - Delphi
tags:
    - Citrix
    - Delphi
    - 'Web Interface'
    - XML
---

After figuring out how to [encode and decode the Citrix passwords](http://192.168.40.25:8081/2012/05/13/encoding-and-decoding-citrix-passwords/) my next step for the upcoming Citrix Launcher is experiment with config.xml and authenticating to the Citrix Web Interface.

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/05/image_thumb4.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/05/image4.png)I imported the NFuse.dtd from the Citrix Web Interface into Delphi with the XML Data Binding Wizard. The results in an NFuse Unit so I can easily create the XML data.

To create an authentication packet I use the following code:

  
“&gt;  
And this produces the following XML:  
“&gt;