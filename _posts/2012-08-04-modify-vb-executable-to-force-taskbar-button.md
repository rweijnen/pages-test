---
id: 2693
title: 'Modify VB Executable to force Taskbar Button'
date: '2012-08-04T11:07:18+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2693'
permalink: /2012/08/04/modify-vb-executable-to-force-taskbar-button/
views:
    - '2688'
short-url:
    - 'http://bit.ly/MEywIr'
categories:
    - Citrix
    - General
tags:
    - Citrix
    - 'Reverse Engineering'
    - 'Visual Basic'
    - XenApp
---

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/08/image_thumb.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/08/image.png)I had to troubleshoot an application that was published with Citrix XenApp. The problem with this application was that it didn’t have an button/icon in the taskbar and the window would sometimes disappear.

I noticed that this (cr)application was written in Visual Basic, so I decided to run it through a [decompilation tool](http://www.vb-decompiler.org/).

The decompiler was able to list the forms used in the Application:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/08/image_thumb1.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/08/image1.png)

I inspected the main form and it showed the following properties:

 “&gt;

Obviously I wanted to change ShowInTaskbar from False to True but the decompiler didn’t have an edit option.

I enabled the following option (Tools | Options):

[![SNAGHTML4ddea108](http://192.168.40.25:8081/wp-content/uploads/2012/08/SNAGHTML4ddea108_thumb.png "SNAGHTML4ddea108")](http://192.168.40.25:8081/wp-content/uploads/2012/08/SNAGHTML4ddea108.png)

This made the Decompiler show the Hex Offset of the form in the exe file:

“&gt;

I opened the exe file in an Hex Editor and jumped to the form Offset. A Visual Basic form is a binary, undocumented format but in the Hex Viewer we can see how it’s structured. Taking the Form Caption (Title) as example we can see that it has Id 1, has a length of 0C (12) Bytes and the String is “Main Form”:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/08/image_thumb2.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/08/image2.png)

Luckily the author of the VB Decompiler has already done the hard work and has created a table of all the Id’s [here](http://www.vb-decompiler.org/forms_editing.htm).

This is a small part of the table, listing the properties I needed:

“&gt;

I was interested in the ShowInTaskbar property (Id 44) and to easily identify it I combined it with the StartupPosition (Id 46). So I had to search for Id 44 followed by False (0 in VB), followed by 46, followed by 02. So I searched for Hex Bytes **44 00 46 02** and found them at offset 0x143014:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/08/image_thumb3.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/08/image3.png)

I needed to change Id 44 to True which is -1 in Visual Basic, the Byte value for -1 is FF so I changed it to **44 FF 46 02**:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/08/image_thumb4.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/08/image4.png)

I saved the file and ran it through the decompiler again to verify the results:

“&gt;

Finally I tested the application and it worked fine now!