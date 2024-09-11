---
id: 2751
title: 'Convert an IP Address to an Integer in C# in host byte ordering'
date: '2012-10-13T15:18:26+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2751'
permalink: /2012/10/13/convert-an-ip-address-to-an-integer-in-c-in-host-byte-ordering/
short-url:
    - 'http://bit.ly/PtIxOh'
views:
    - '5410'
categories:
    - 'C#'
tags:
    - .NET
    - '.NET FrameWork'
    - 'C#'
---

Yesterday I wrote about [converting an IP Address to an Integer in C#](http://192.168.40.25:8081/2012/10/12/warning-cs0618-system-net-ipaddress-address-is-obsolete/). But both methods I presented return the IP Address in network byte order.

However in some cases, especially when calling WinApi functions, you will need to convert the Integer to host byte order which is *little-endian* on Intel processors.

In an unmanaged language we could do very fast byte swap with inline assembly, eg:  
“&gt;  
From WinApi we could use the [ntohl](http://msdn.microsoft.com/en-us/library/windows/desktop/ms740069%28v=vs.85%29.aspx) function and in managed languages we can use the [NetworkToHostOrder](http://msdn.microsoft.com/en-us/library/653kcke1.aspx) method from the [System.Net.IPAddress](http://msdn.microsoft.com/en-us/library/system.net.ipaddress.aspx) class.

For an IPv4 address we need to make sure we are using the proper overload by casting the result of System.BitConverter to an int:  
“&gt;