---
id: 822
title: 'Citrix Web Interface starts very slowly'
date: '2010-11-26T15:07:45+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=822'
permalink: /2010/11/26/citrix-web-interface-starts-very-slowly/
views:
    - '2204'
short-url:
    - 'http://bit.ly/gfjaew'
categories:
    - Altiris
    - Citrix
    - PowerShell
    - script
---

I remembered from a previous project that when the Citrix Web Interface this is caused by a setting called *generatePublisherEvidence* in the Aspnet.config file.

This behaviour has been documented by Citrix in [CTX117273](http://support.citrix.com/article/ctx117273 "Web Interface 5.x Delay on First Page").

If you read it carefully you will see the note that you need to fix it in 2 places for an x64 system.

If you know me a little than you have probably guessed I wanted to fix this with a nice script. I have chooses PowerShell this time because it has good support for XML and I made a one-liner so I can easily use it in an Embedded Altiris script.

The script changes the config file for both x86 and x64:  
“&gt;  
**EDIT:** If you run by commandline you need to care of *quotes within quotes*, easiest thing to do is to use *double* quotes (“) to surround the commandline and use *single* quotes (‘) for Strings inside the commandline:  
“&gt;