---
id: 4061
title: 'Modifying a .NET Application'
date: '2017-03-14T15:31:03+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://192.168.40.25:8081/?p=4061'
permalink: /2017/03/14/modifying-net-application/
views:
    - '1009'
categories:
    - .NET
    - Apple
tags:
    - Apple
    - BootCamp
    - Drivers
    - Intel
    - JustDecompile
    - Reflexil
    - Telerik
    - Thunderbolt
---

I will explain why in a seperate post, but on my MacBook Pro I wanted to use the Intel Thunderbolt driver under BootCamp instead of the ones supplied by Apple.

The Thunderbolt control program however refused with the following error message:

[![This application is not supported on Boot Camp. (Thunderbolt devices and networking will work correctly.)](http://192.168.40.25:8081/wp-content/uploads/2017/03/image_thumb-5.png "Application Cannot Run")](http://192.168.40.25:8081/wp-content/uploads/2017/03/image-5.png)

It’s really beyond me why Intel would deliberately block their Thunderbolt software on Apple hardware (under Windows). Believing this was just a simple hardcoded hardware check rather than any hardware issue that would prevent the drivers to work I proceeded into finding where the check takes place.

Total Commander is a nice tool for this as it allows you to search for text in folders (recursively) in different encodings (ASCII, Unicode etc).

I searched for the text “Boot Camp” in the folder `C:\\Program Files (x86)\\Intel\\Thunderbolt Software` and found the exact text from the error message:

[![Total Commander | Find Files | ASCII | Unicode](http://192.168.40.25:8081/wp-content/uploads/2017/03/image_thumb-6.png "Find Files")](http://192.168.40.25:8081/wp-content/uploads/2017/03/image-6.png)

Ignoring the resources dll’s as I figured these would only contain translations in different languages I had a look at `Thunderbolt.exe`.

Since managed and non managed binaries require different tools for analysis and patching my first step was to determine if the binary was managed or non managed.

Presence of the file `Thunderbolt.exe.config` already hinted at managed and the contents of the file confirmed it:  
“&gt;

I already wrote a blog post on how to modify a managed binary with [Reflector and Reflexil](http://192.168.40.25:8081/2013/05/30/redirect-registry-by-modifying-net-executable/) in 2013 but today other and easier ways to do this exist.

For this blog post I am using [Telerik JustDecompile](http://www.telerik.com/products/decompiler.aspx) which has a great feature: you can replace code in the binary with code rather than .NET IL (Intermediate Language). That makes it a lot easier!

Open the `Thunderbolt.exe`file in JustDecompile and press the Search button. I first searched for `Boot Camp` and `BootCamp` which both yielded no result. Next search was `Apple`which was a hit:

[![Telerik JustDecompile | Search | Apple](http://192.168.40.25:8081/wp-content/uploads/2017/03/image_thumb-7.png "Search")](http://192.168.40.25:8081/wp-content/uploads/2017/03/image-7.png)

The Method `IsAppleSystem`sounds like exactly where we need to be so let’s click that one:

Here is the code:  
“&gt;

It uses WMI (which is evil but that’s a seperate discussion) and checks the manufacturer string. Let’s change this function to always return false!

Choose Reflexil from the Plugins menu:

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/03/image_thumb-8.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/03/image-8.png)

This brings up the Reflexil window with the IL code. Simple Right Click and select `Replace all with code.`:

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/03/image_thumb-9.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/03/image-9.png)

This will show the `Compile`screen:

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/03/image_thumb-10.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/03/image-10.png)

Simple replace `return default(bool);` with `return false;`and press `Compile`followed by `Ok`.

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/03/image_thumb-11.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/03/image-11.png)

Finally save the modified binary:

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/03/image_thumb-12.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/03/image-12.png)

And voila that did it!

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/03/image_thumb-13.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/03/image-13.png)

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/03/image_thumb-14.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/03/image-14.png)

No security doesn’t look the most secure level (so much for security eh Apple?):

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/03/image_thumb-15.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/03/image-15.png)