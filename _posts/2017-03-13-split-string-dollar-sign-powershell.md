---
id: 4015
title: 'Split a string by dollar sign ($) in PowerShell'
date: '2017-03-13T15:28:13+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://192.168.40.25:8081/?p=4015'
permalink: /2017/03/13/split-string-dollar-sign-powershell/
views:
    - '436'
categories:
    - PowerShell
tags:
    - PowerShell
    - Split
---

Just a very quick post (more like a note to self) but I wanted to split a string with the $ sign in PowerShell:  
“&gt;  
Took me a little while to realize that this doesn’t work as the [split](https://msdn.microsoft.com/en-us/powershell/reference/3.0/microsoft.powershell.core/about/about_split) operator in Windows PowerShell uses a regular expression in the delimiter, rather than a simple character.

The easy fix is to Escape the $ sign with a backslash:  
“&gt;  
Or alternatively use the SimpleMatch option:  
“&gt;  
The 0 represents the “return all” value of the Max-substrings parameter. You can use options, such as SimpleMatch, only when the Max-substrings value is specified.