---
id: 2725
title: 'Calling a function inside another assembly in .NET'
date: '2012-08-22T09:42:06+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2725'
permalink: /2012/08/22/calling-a-function-inside-another-assembly-in-net/
short-url:
    - 'http://bit.ly/PWgLsD'
views:
    - '1636'
categories:
    - 'C#'
---

![Red Gate .NET Reflector](http://www.apponic.com/data/softimg/windows/development/net/red-gate-net-reflector-95603-icon.jpeg)I wanted to call a hash function from a .net executable from my code. My first step was to inspect the executable with [Reflector](http://www.reflector.net/).

The Hash function was in a namespace called Core:

“&gt;

Notice that the Core namespace is marked as internal so it was not meant to be callable outside of the executable. It’s still possible to call it using Reflection:

  
“&gt;

This works nicely! However if the function you want to call is overloaded (eg if there would also be a Hash function accepting 3 parameters) then you need to specify which overload you want:

“&gt;

Now that was easy, wasn’t it?