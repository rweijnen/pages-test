---
id: 2745
title: 'warning CS0618: &#8216;System.Net.IPAddress.Address&#8217; is obsolete'
date: '2012-10-12T13:43:21+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2745'
permalink: /2012/10/12/warning-cs0618-system-net-ipaddress-address-is-obsolete/
short-url:
    - 'http://bit.ly/TFIrUu'
views:
    - '4250'
categories:
    - 'C#'
tags:
    - .NET
    - '.NET FrameWork'
    - 'C#'
---

For a call to a WinApi function I needed to convert an IP Address to an Integer in C#.

This can be done using the [System.Net.IPAddress](http://msdn.microsoft.com/en-us/library/system.net.ipaddress.aspx) class:

“&gt;

Although this works, the compiler issues a warning:

warning CS0618: ‘System.Net.IPAddress.Address’ is obsolete: ‘This property has been deprecated. It is address family dependent. Please use IPAddress.Equals method to perform comparisons. [http://go.microsoft.com/fwlink/?linkid=14202′](http://go.microsoft.com/fwlink/?linkid=14202')

This warning is issued because the Address property is not IPv6 compatible. The warning can be suppressed like this:

“&gt;

But it would be better to use the non deprecated GetAddressBytes() Method:

“&gt;