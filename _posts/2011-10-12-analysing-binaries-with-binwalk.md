---
id: 2136
title: 'Analysing binaries with Binwalk'
date: '2011-10-12T16:24:50+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/10/12/analysing-binaries-with-binwalk/'
permalink: /2011/10/12/analysing-binaries-with-binwalk/
views:
    - '10587'
short-url:
    - 'http://bit.ly/oBYfWa'
categories:
    - Embedded
---

[![](http://1.bp.blogspot.com/-ntyKMAnYB84/TjA2kMXPo-I/AAAAAAAACmA/OKjXuz3HORU/s640/software_test_web.jpg)](http://1.bp.blogspot.com/-ntyKMAnYB84/TjA2kMXPo-I/AAAAAAAACmA/OKjXuz3HORU/s1600/software_test_web.jpg)I came across an interesting tool today called [Binwalk](http://code.google.com/p/binwalk/).   
Binwalk is a firmware analysis tool that scans a given binary file for embedded files and executable code.

![](data:image/jpg;base64,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)Binwalk requires a Linux machine, I used the Backtrack VM I used from my [article about WEP keys](http://192.168.40.25:8081/2011/06/14/crack-wep-encryption/).

Note there is no binary distribution of Binwalk so you will need to compile it but this is a breeze.

Unpack the downloaded version (I used 0.3.10 which is the most recent at this time).

 “&gt;  
Binwalk comes with a configure script that checks the preconditions and creates a make file. Start if from the src directory:

“&gt;  
If it complains about libcurl, install it:

“&gt;  
Then compile it:

“&gt;

Now analyse your binaries with binwalk &lt;filename&gt;. Here is some example output:

“&gt;

The [Binwalk wiki](http://code.google.com/p/binwalk/w/list) contains some [usage examples](http://code.google.com/p/binwalk/wiki/Usage) to get you started.