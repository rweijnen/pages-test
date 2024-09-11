---
id: 2634
title: 'Decoding Citrix IMA Datastore Password'
date: '2012-05-29T21:46:30+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2634'
permalink: /2012/05/29/decoding-citrix-ima-datastore-password/
short-url:
    - 'http://bit.ly/KYdjqR'
views:
    - '5127'
categories:
    - Citrix
    - Delphi
    - Programming
tags:
    - Citrix
    - datastore
    - Decode
    - Decrypt
    - IMA
    - Password
    - Username
---

This morning [Arjan Beijer](http://www.twitter.com/arbeijer/status/207398601066942464) sent me an interesting link to a [youtube video](http://www.youtube.com/watch?v=Cb8-kvJkojY) about obtaining the Citrix IMA Datastore password using Windbg.

The video shows a method, discovered by [Denis Gundarev](https://twitter.com/#!/fdwl) to obtain the IMA Datastore password. Basically he uses DSMaint.exe and set’s a breakpoint on the call to [CryptUnprotectData](http://msdn.microsoft.com/en-us/library/windows/desktop/aa380882(v=vs.85).aspx) and then reads the password from memory.

I tried to call the CryptUnprotectData API with the data read from the registry directly but this failed with error NTE\_BAD\_KEY\_STATE, this is defined in winerror.h and it means “Key not valid for use in specified state”.

### Entropy

I assumed Citrix was using an Entropy (salt) to make the decoding a little more difficult so I checked the disassembly from DSMaint with Ida Pro and it imports a function called GlobalData\_GetDecryptedStrW from ImaSystem.dll:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/05/image_thumb6.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/05/image6.png)

I openend ImaSystem.dll in Ida Pro and found CryptUnprotectData in the Imports Tab:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/05/image_thumb7.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/05/image7.png)

I checked the references (Ctrl-X) and went to the one on the top of the list:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/05/image_thumb8.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/05/image8.png)

I don’t think it’s difficult to spot the Entropy here?

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/05/image_thumb9.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/05/image9.png)

### Code

The code needed to decrypt the password is just a few lines:  
“&gt;

### Tool

At the bottom of this post is a downloadable tool that reads the username and password data from the registry, decrypts and displays it:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/05/image_thumb10.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/05/image10.png)

### Security Breach?

But where does this leave us? Is it a security breach?

I don’t think so, since the call to CryptUnprotectData fails if we do not have Admin privileges. Further more we can read the values remotely (if we have admin privileges) but we can only decrypt it locally.

[ Citrix IMA DataStore Username &amp; Password Decoder (2338 downloads ) ](http://192.168.40.25:8081/download/citrix-ima-datastore-username-password-decoder/?tmstv=1726048919 "Version 1.0")