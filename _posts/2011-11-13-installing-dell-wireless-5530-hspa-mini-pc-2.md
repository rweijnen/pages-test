---
id: 2180
title: 'Installing Dell Wireless 5530 HSPA Mini PC #2'
date: '2011-11-13T20:20:51+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2180'
permalink: /2011/11/13/installing-dell-wireless-5530-hspa-mini-pc-2/
short-url:
    - 'http://bit.ly/sVRq6v'
views:
    - '17125'
categories:
    - General
tags:
    - Dell
---

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/11/image_thumb3.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/11/image3.png)In a Comment on my Article “[Installing Dell Wireless 5530 HSPA Mini PCI](http://192.168.40.25:8081/2011/05/27/installing-dell-wireless-5530-hspa-mini-pci/)“, Florian asked how to Install Dell’s [R298998](http://support.dell.com/support/downloads/download.aspx?c=us&cs=19&l=en&s=dhs&releaseid=R298998&SystemID=VOS_N_3450&servicetag=&os=W764%20&osl=en&deviceid=25633&devlib=0&typecnt=0&vercnt=1&catid=-1&impid=-1&formatcnt=0&libid=20&typeid=-1&dateid=-1&formatid=-1&source=-1&fileid=448952) driver on non authorized system and card combinations.

I decided to have a look and downloaded this driver. The structure isn’t much different from the R251153 driver I described in my earlier post.

When installing it on a non authorized card/laptop combination the error is similar:

[![Authentification failed. The Dell Wireless HSPA Mobile Broadband Mini-Card cannot be installed on this computer. Please contact the Dell support for further information.](http://192.168.40.25:8081/wp-content/uploads/2011/11/SNAGHTML107d7cc3_thumb.png "Dell Wireless HSPA Mini-Card Drivers")](http://192.168.40.25:8081/wp-content/uploads/2011/11/SNAGHTML107d7cc3.png)

When the error message appeared I looked into the temp folder and I noticed that just like before 2 folders were created (with a GUID as name). One of these folders contained the file *driver\_auth.exe* which, as I already knew, performs the actual validation.

I loaded driver\_auth.exe in Ida and in the Strings windows I searched for “dell\_wwan\_sysID.dat”. As I described earlier this is an encrypted file that contains the list of authorized combinations.

[![Ida Strings Window | dell_wwan_sysID.dat](http://192.168.40.25:8081/wp-content/uploads/2011/11/image_thumb4.png "Strings window")](http://192.168.40.25:8081/wp-content/uploads/2011/11/image4.png)

There were only 2 references (Ctrl-X) to this string:

[![References to dell_wwan_sysID.dat string](http://192.168.40.25:8081/wp-content/uploads/2011/11/SNAGHTML108af58b_thumb.png "xrefs to dell_wwan_sysID.dat")](http://192.168.40.25:8081/wp-content/uploads/2011/11/SNAGHTML108af58b.png)

This reference (last line in the screenshot below) brings us to the right place because a little above it is a debug output line that says: “Decrypting dell\_wwan\_sysID.dat into dell\_wwan\_sysID.txt”:

[![Ida Code Snippet](http://192.168.40.25:8081/wp-content/uploads/2011/11/image_thumb5.png "Code Snippet")](http://192.168.40.25:8081/wp-content/uploads/2011/11/image5.png)

A little above in the code is a Switch statement that operations on the first commandline argument:

[![jmp ds:off_403e44[ecx*4]](http://192.168.40.25:8081/wp-content/uploads/2011/11/image_thumb6.png "switch jump")](http://192.168.40.25:8081/wp-content/uploads/2011/11/image6.png)

On case 68 or 76 a password check (password is expected as 2nd parameter) is done (notice that ASCII 68 = ‘D’ and 76 = ‘L’):

[![jnz short loc_403B58](http://192.168.40.25:8081/wp-content/uploads/2011/11/image_thumb7.png "Password Check")](http://192.168.40.25:8081/wp-content/uploads/2011/11/image7.png)

We could of course patch jzn (Jump when Not Zero) to jz (Jump when Zero) to accept any password but I used Ida’s integrated debugger to read the password.

Oh you want to know what the password is?

It’s “**HELMSLEY**” (in capitals).

The following commandlines are possible:

 “&gt;

So let’s decrypt the file:

“&gt;

So how do we know what line we need to modify?

Simple, use the -U switch, in my case I have a D5530 card so I use the following commandline:

“&gt;

[![SNAGHTML109f370e](http://192.168.40.25:8081/wp-content/uploads/2011/11/SNAGHTML109f370e_thumb.png "SNAGHTML109f370e")](http://192.168.40.25:8081/wp-content/uploads/2011/11/SNAGHTML109f370e.png)

So my system has ID 1036, according to the list ONLY the combination of 1036 and Dell 5540 is authorized

“&gt;

So we change it and add the D5530:

“&gt;

Save the file and encrypt it:

“&gt;

And let’s check again:

[![SNAGHTML10aa79d8](http://192.168.40.25:8081/wp-content/uploads/2011/11/SNAGHTML10aa79d8_thumb.png "SNAGHTML10aa79d8")](http://192.168.40.25:8081/wp-content/uploads/2011/11/SNAGHTML10aa79d8.png)

And no more Error:

[![SNAGHTML10ab69c1](http://192.168.40.25:8081/wp-content/uploads/2011/11/SNAGHTML10ab69c1_thumb.png "SNAGHTML10ab69c1")](http://192.168.40.25:8081/wp-content/uploads/2011/11/SNAGHTML10ab69c1.png)

If you are not sure which card you have then use Process Monitor during the installation with the following Filter:

[![SNAGHTML10b433c4](http://192.168.40.25:8081/wp-content/uploads/2011/11/SNAGHTML10b433c4_thumb.png "SNAGHTML10b433c4")](http://192.168.40.25:8081/wp-content/uploads/2011/11/SNAGHTML10b433c4.png)

Use the last parameter in the Commandline:

[![SNAGHTML10b50371](http://192.168.40.25:8081/wp-content/uploads/2011/11/SNAGHTML10b50371_thumb.png "SNAGHTML10b50371")](http://192.168.40.25:8081/wp-content/uploads/2011/11/SNAGHTML10b50371.png)

If this article was helpful to you please leave a comment.