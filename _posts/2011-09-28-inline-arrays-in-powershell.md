---
id: 2119
title: 'Inline arrays in PowerShell'
date: '2011-09-28T10:34:18+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/09/28/inline-arrays-in-powershell/'
permalink: /2011/09/28/inline-arrays-in-powershell/
views:
    - '1934'
short-url:
    - 'http://bit.ly/rjivEo'
categories:
    - PowerShell
tags:
    - PowerShell
---

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/09/image_thumb28.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/09/image28.png)Sometimes I want to process a list of “things” easily in PowerShell where the list is not in an external file but in the script itself.

Ideally this list would not be separated by e.g. a comma so it can be easily copy/pasted from external data sources.

Something like this:  
“&gt;

With a little trickery we can!  
“&gt;