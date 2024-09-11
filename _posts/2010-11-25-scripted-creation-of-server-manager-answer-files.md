---
id: 807
title: 'Scripted creation of Server Manager Answer Files'
date: '2010-11-25T16:14:42+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=807'
permalink: /2010/11/25/scripted-creation-of-server-manager-answer-files/
views:
    - '1844'
short-url:
    - 'http://bit.ly/hhhC80'
categories:
    - Altiris
    - Exchange
    - script
    - 'Windows 2008'
---

A while ago I created a script that I can run as embedded script in Altiris that creates a Server Manager Answer File (for Server 2008).

I could have simply done an echo &gt;answer.xml but I wanted a well formed XML that could be read and displayed in an XML editor or Internet Explorer when needed.

I use the Microsoft.XMLDom object in the script the create the XML and I think the code is easy to understand so I will just show it here:  
“&gt;  
Sample output:

[![AnswerXML](http://192.168.40.25:8081/wp-content/uploads/2010/11/answerxml-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/11/answerxml.png)