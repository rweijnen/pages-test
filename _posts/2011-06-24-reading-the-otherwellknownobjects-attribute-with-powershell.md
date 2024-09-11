---
id: 1899
title: 'Reading the otherWellKnownObjects attribute with PowerShell'
date: '2011-06-24T11:19:25+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/06/24/reading-the-otherwellknownobjects-attribute-with-powershell/'
permalink: /2011/06/24/reading-the-otherwellknownobjects-attribute-with-powershell/
views:
    - '4840'
short-url:
    - 'http://bit.ly/iLfJhF'
categories:
    - 'Active Directory'
    - PowerShell
tags:
    - 'Active Directory'
    - otherWellKnownObjects
    - PowerShell
    - Reflection
---

I wanted to read the [otherWellKnownObjects](http://msdn.microsoft.com/en-us/library/ms679095(v=vs.85).aspx) attribute from an Active Directory object.

In my case this was the Microsoft Exchange container in the Configuration partition:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/06/image_thumb14.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/06/image14.png)

The otherWellKnownObjects attribute is of type ADSTYPE\_DN\_WITH\_BINARY which unfortunately cannot be viewed or edited with ADSI Edit:

[![There is no editor registered to handle this attribute type](http://192.168.40.25:8081/wp-content/uploads/2011/06/image_thumb15.png "ADSI Edit")](http://192.168.40.25:8081/wp-content/uploads/2011/06/image15.png)

So I wrote a script to read the values with PowerShell but it’s not very straightforward how to do this.

Reading the otherWellKnownObjects property returns a collection of [IADsDNWithBinary](http://msdn.microsoft.com/en-us/library/aa705996(VS.85).aspx) interfaces but this interface is unknown in PowerShell.

Using reflection we can read the values:

 “&gt;