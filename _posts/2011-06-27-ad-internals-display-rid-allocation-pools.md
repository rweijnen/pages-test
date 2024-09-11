---
id: 1950
title: 'AD Internals: Display RID Allocation Pools'
date: '2011-06-27T15:37:04+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1950'
permalink: /2011/06/27/ad-internals-display-rid-allocation-pools/
short-url:
    - 'http://bit.ly/kR38Kk'
views:
    - '8342'
categories:
    - 'Active Directory'
    - PowerShell
    - Programming
tags:
    - 'Active Directory'
    - 'ADSI Edit'
    - Internals
    - PowerShell
---

In my previous post I wrote about a problem I had with duplicate RID Allocation pools.

But how do we get more insight into these RID Allocation pools?

The DCDIAG tool can display this information per domain controleler using the following syntax

 “&gt;

Example output:

[![DCDiag Ridmanager Test](http://192.168.40.25:8081/wp-content/uploads/2011/06/image_thumb28.png "DCDiag Ridmanager Test")](http://192.168.40.25:8081/wp-content/uploads/2011/06/image28.png)

But where in Active Directory is this information stored and can we display it for all Domain Controllers at once for larger environments?

Let’s start with the Active Directory part, the *System* container has an object named *RID Manager$:*

[![ADSI Edit System Container](http://192.168.40.25:8081/wp-content/uploads/2011/06/image_thumb29.png "ADSI Edit")](http://192.168.40.25:8081/wp-content/uploads/2011/06/image29.png)

The *fSMORoleOwner* attribute holds the RID Master FSMO role owner.

*rIDAvailablePool* is a Large Integer (an 8 byte value) where the lower 4 bytes are the From (Beginning of next RID pool to be allocated) and the higher 4 bytes are the To (Total number of RIDS that can be created in a domain) as displayed by *dcdiag*.

The Allocation Pools and the Next RID are kept by each server in a child object called RID Set. We can find the RID Set by querying the *rIDSetReferences* attribute which contains the LDAP path to the RID Set:

[![rIDSetReferences Attribute](http://192.168.40.25:8081/wp-content/uploads/2011/06/image_thumb30.png "ADSI Edit")](http://192.168.40.25:8081/wp-content/uploads/2011/06/image30.png)

The RID Set contains the other values we are looking for where *rIDAllocationpool* (the pool currently in use) and *rIDPreviousAllocationpool* (the pool that will be used next when the current pool is exhausted) are again Large Integers with a Low and a High part:

[![RID Set Properties](http://192.168.40.25:8081/wp-content/uploads/2011/06/image_thumb31.png "ADSI Edit")](http://192.168.40.25:8081/wp-content/uploads/2011/06/image31.png)

Now that we know where the values are stored we can write a script, I have chosen PowerShell.

First we connect to the (Default) domain and obtain the distinguishedName of the domain (DC=MyDomain, DC=local).

“&gt;

Now we can open RID Manager Object:

“&gt;

And query for the FSMO Role Owner:

“&gt;

From the RID Master we read the rIDAvailablePool attribute:

“&gt;

GetInteger8 is a helper function to read Integer8 (Large Integer) values from Active Directory:

“&gt;

We are going to store all RID Data in an array so we can use the Format options from PowerShell:

“&gt;

I wrote a function to gather and return the RID Data for a Domain Controller object:

“&gt;

Now we can Bind to the Domain Controllers OU, enumerate all children and gather the RID Data for them:

“&gt;

Last step is outputting the Data:

“&gt;

This is the data for my environment:

[![RID Data for the Domain](http://192.168.40.25:8081/wp-content/uploads/2011/06/image_thumb32.png "PowerShell Console")](http://192.168.40.25:8081/wp-content/uploads/2011/06/image32.png)

The complete script can be downloaded below.

[ rIDump (1828 downloads ) ](http://192.168.40.25:8081/download/ridump/?tmstv=1726048919 "Version 1.0")