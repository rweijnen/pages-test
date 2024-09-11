---
id: 3182
title: 'RC4 Encryption in PowerShell'
date: '2013-04-05T16:27:32+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3182'
permalink: /2013/04/05/rc4-encryption-in-powershell/
views:
    - '2252'
short-url:
    - 'http://bit.ly/Zcw3Jh'
categories:
    - PowerShell
---

For an upcoming blog post I needed to decrypt some data using the [rc4 algorithm](http://en.wikipedia.org/wiki/RC4). I wanted to do this with PowerShell but sadly PowerShell and the .NET framework have no functions for it.

![File:RC4.svg](http://upload.wikimedia.org/wikipedia/commons/thumb/e/e9/RC4.svg/800px-RC4.svg.png)

So I needed to implement it (download at the bottom of the post):

  
“&gt;

And here is a usage example (the BinToHex and HexToBin functions are from my [previous blog](http://192.168.40.25:8081/2013/04/05/convert-bin-to-hex-and-hex-to-bin-in-powershell/)).

“&gt;

[ rc4.zip (4328 downloads ) ](http://192.168.40.25:8081/download/rc4-zip/?tmstv=1726048920 "Version 1.0")