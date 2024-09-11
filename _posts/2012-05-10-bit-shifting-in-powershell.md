---
id: 2575
title: 'Bit Shifting in PowerShell'
date: '2012-05-10T12:14:36+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2575'
permalink: /2012/05/10/bit-shifting-in-powershell/
short-url:
    - 'http://bit.ly/KHIrzY'
views:
    - '4227'
categories:
    - PowerShell
tags:
    - 'Bit Shifting'
    - PowerShell
    - shl
    - shr
---

![](http://upload.wikimedia.org/wikipedia/commons/thumb/5/5c/Rotate_left_logically.svg/210px-Rotate_left_logically.svg.png)I needed to dome some [Bit Shifting](http://en.wikipedia.org/wiki/Bitwise_operation#Logical_shift) in PowerShell but unfortunately PowerShell lacks operator for Bit Shifting. I searched the .NET Framework for anything that allows for bit shifting but was unable to find anything suitable.

I didn’t want to revert to C# so I implemented shift left and shift right functions in PowerShell.

The code isn’t really pretty and could probably be improved (comments/improvements are welcome!) but here goes (please note that I implemented for bit shifting a byte):

“&gt;