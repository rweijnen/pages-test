---
id: 332
title: 'Reading physical memory size from the registry'
date: '2009-03-20T13:15:46+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2009/03/20/reading-physical-memory-size-from-the-registry/'
permalink: /2009/03/20/reading-physical-memory-size-from-the-registry/
views:
    - '13733'
short-url:
    - 'http://bit.ly/hMuSoH'
categories:
    - Delphi
    - Programming
tags:
    - TSAdminEx
---

I’m working on a new build of [TSAdminEx](http://192.168.40.25:8081/2009/02/20/tsadminex-beta-release/) for which I need to query the total amount of physical memory. Locally we can use the [GlobalMemoryStatusEx](http://msdn.microsoft.com/en-us/library/aa366589(VS.85).aspx) API but there’s no API to do this remotely. It would be possible using WMI but I decided not to use that because I dislike it because of it’s slowness and I need support for older OS versions which might not have WMI.

So I found in the registry the following key:

> HKLM\\HARDWARE\\RESOURCEMAP\\System Resources\\Physical Memory

It has a value .Translated of type RES\_RESOURCE\_LIST which seems undocumented besides stating that it exists. Regedit knows how to handle it though. If you doubleclick on the key you will see something like this:

[![ResourceLists](http://192.168.40.25:8081/wp-content/uploads/2009/03/resourcelists-1-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/03/resourcelists-1.png)

If we click on the list and choose display we see the following:

[![Resources](http://192.168.40.25:8081/wp-content/uploads/2009/03/resources-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/03/resources.png)

If we sum the Length of all memory blocks we get the total amount of Physical Memory. I wondered how to do this programmatically so I Googled on it but found mostly pages where the values are extracted at fixed offsets rather than knowing the correct structure. I found however that the structure is present in the DDK (wdm.h) and is called CM\_RESOURCE\_LIST. I was also lucky because [Jack van Nuenen](http://www.vns.nl/?source=data/xp485/EnumCom.pas) had already done a conversion of those headers to Delphi which saved me some work. I did some small modifications to the headers and included them in a new unit into the Jedi Apilib (JwaWdm.pas).

Let’s take a look at the structure:  
“&gt;  
ANYSIZE\_ARRAY is a constant from the Windows PSDK which is meant to make a dummy array, so here we make actually an array\[0..0\]. However the compiler permits us to do array\[1\], array\[2\] if we turn off range checking so this makes it easier to loop through the array.

The CM\_FULL\_RESOURCE\_DESCRIPTOR describes the resource:  
“&gt;  
And finally points to a CM\_PARTIAL\_RESOURCE\_LIST structure:  
“&gt;  
Which in turn points to an array of CM\_PARTIAL\_RESOURCE\_DESCRIPTOR structures:  
“&gt;  
We are interested in the RDD\_MEMORY structure which finally gives us the requested information (Length):  
“&gt;  
So it’s just a matter of looping through the Partial Descriptor array and summing up the Lengths. However on a x64 (64 bit) Windows the memory length is a 8 bytes instead of 4 byte, so we need to account for that. If we use a 64 bit compiler it will handle this automatically because the ULONG type is 8 bytes then. That will make it impossible though to query remote 32 bit systems from 64 bit or vice versa. So I wondered how Taskmanager handled that, the answer is: IT DOESN’T! See the following example:

Querying locally on 32 bit system:

![x86Local](http://192.168.40.25:8081/wp-content/uploads/2009/03/x86local.png)

Now I query the same machine remotely from a 64 bit system:

![x86Remote](http://192.168.40.25:8081/wp-content/uploads/2009/03/x86remote.png)

I think it can only be solved by using two different structures, one for x86 and one for x64. So I introduced the following structures:  
“&gt;  
Now we need a generic function to query both x86 and x64 (I use the Environment variable PROCESSOR\_ARCHITECTURE to determine if a system is 64bit):  
“&gt;