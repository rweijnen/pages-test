---
id: 879
title: 'Packaging an application – Auto Accept the EULA'
date: '2010-12-17T17:10:03+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=879'
permalink: /2010/12/17/packaging-an-application-auto-accept-the-eula/
views:
    - '1977'
short-url:
    - 'http://bit.ly/gBWDpL'
categories:
    - Packaging
    - PowerShell
    - script
    - 'Unattended Installation'
---

Yesterday I was packaging an application called Kluwer Juridische Bibliotheek. When the user first starts this application a screen with the License Conditions pops up and it must be accepted:

[![KluwerEULA](http://192.168.40.25:8081/wp-content/uploads/2010/12/kluwereula-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/12/kluwereula.png)

I always try to remove such things as I don’t think it’s necessary for every user to accept it.

So I took the usual approach and used Process Monitor to see where acceptance of this agreement is recorded.

This application creates at file called CMUSER.CFG in the Start Directory of the Application. It’s a binary file of 276 bytes, probably some kind of struct/record written to a file.

I inspected it with an Hex Editor and looked for a byte with the value 1 (the ordinal value of a Boolean) and there were only two.

One of them was followed by three times 00:  
[![HexDump](http://192.168.40.25:8081/wp-content/uploads/2010/12/hexdump-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/12/hexdump.png)  
This is the most likely suspect because often struct/record members are aligned on 4 byte boundaries or a Boolean type is 4 byte.

I verified this by changing it to 0 and then the EULA is shown again.

I decided to add this to the login script because it’s very little effort to do so (I use a very clever login script). I just need to make a folder with the application’s group name in the groups subfolder and place any script in there.

The logonscript knows that it only has to inspect this folder if the user is a member of this group and knows how to handle different file types (eg a reg file is imported but a vbs or ps1 file is executed).

In my case I created a PowerShell file because I can embed a byte dump of the CMUSER.CFG file in it:  
“&gt;