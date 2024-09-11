---
id: 588
title: 'Having fun with Windows Licensing'
date: '2010-06-15T15:09:14+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=588'
permalink: /2010/06/15/having-fun-with-windows-licensing/
views:
    - '8699'
short-url:
    - 'http://bit.ly/hwXKmZ'
categories:
    - Programming
    - Vista
    - 'Windows 7'
---

If you look into the registry in the key HKLM\\System\\CurrentControlSet\\ProductOptions you will find several licensing related Values.

The ProductType and ProductSuite keys contain the OS Suite and Edition, but the ProductPolicy key is much more interesting. So let’s have a closer look at it, open RegEdit and DoubleClick the key, you will something like the screenshot below, a Binary Value:

[![ProductPolicy1](http://192.168.40.25:8081/wp-content/uploads/2010/06/productpolicy1-1-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/06/productpolicy1-1.png)

As you can see the license names are there as a Unicode string and later on I will show you how we can read the values. But because I didn’t want to extract all the names manually I decided to see if I could reverse the used structure because it didn’t look very complicated. Using a Hex Editor I could determine the important part of the structure.

It starts with a header:  
“&gt;  
then an array of values follows:  
“&gt;  
The SlDataType is a word value that corresponds to the values of the SLDATATYPE enum:  
“&gt;  
And we end with an End Marker (of size cbEndMarker).

Then I wrote some code to parse the structure:  
“&gt;  
The procedure that reads the actual value is CheckValue, it uses the [SLGetWindowsInformation](http://msdn.microsoft.com/en-us/library/aa965834%28VS.85%29.aspx) function:  
“&gt;  
The results are very interesting, I didn’t know there were so many licensable features in Windows!

You can check your results with the demo project (source included).  
[ License Demo (2115 downloads ) ](http://192.168.40.25:8081/download/license-demo/?tmstv=1726048918 "Version 1.0")

Here are the results from my Windows 7 Laptop:  
[![LicenseValues](http://192.168.40.25:8081/wp-content/uploads/2010/06/licensevalues-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/06/licensevalues.png)