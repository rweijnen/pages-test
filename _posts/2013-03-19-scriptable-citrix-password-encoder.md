---
id: 3152
title: 'Scriptable Citrix Password Encoder'
date: '2013-03-19T16:27:49+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3152'
permalink: /2013/03/19/scriptable-citrix-password-encoder/
short-url:
    - 'http://bit.ly/Ymhh6p'
views:
    - '1871'
categories:
    - Citrix
    - Programming
    - script
tags:
    - Citrix
    - PowerShell
    - Security
    - VBS
---

A while ago I [published](http://192.168.40.25:8081/2012/05/13/encoding-and-decoding-citrix-passwords/) a tool to Encode and Decode Citrix passwords. Today I am publishing a small update to this tool that makes it scriptable by adding a [COM interface](http://en.wikipedia.org/wiki/Component_Object_Model).

If you start the tool without parameters you will get the GUI, just like before:

![Encrypt | Decrypt Password | Hash | Citrix | Ctx1](http:///192.168.40.25:8081/wp-content/uploads/2012/05/image_thumb3.png?resize=419%2C84 "Citrix Password Hasher by Remko Weijnen")

To use the COM interface you first need to register the executable with the /regserver switch:

[![CtxPass /RegServer](http://192.168.40.25:8081/wp-content/uploads/2013/03/SNAGHTML185eb4ec_thumb.png "Register CtxPass")](http://192.168.40.25:8081/wp-content/uploads/2013/03/SNAGHTML185eb4ec.png)

After the registration you can call it using any language that supports COM. To get you started I wrote a few examples

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb32.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image32.png)**<u>PowerShell </u>** “&gt;

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/03/image_thumb33.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/03/image33.png)**<u>VBScript</u>**

“&gt;

**Please note that for security reasons I have chosen to only allow Encoding via the scripting interface.**

[ CtxPassCom.zip (4413 downloads ) ](http://192.168.40.25:8081/download/ctxpasscom-zip/?tmstv=1726048920 "Version 1.0")