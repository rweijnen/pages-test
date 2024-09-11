---
id: 2116
title: 'RNS 510 Startup Logo&ndash;My thoughts'
date: '2011-09-27T22:13:44+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/09/27/rns-510-startup-logomy-thoughts/'
permalink: /2011/09/27/rns-510-startup-logomy-thoughts/
views:
    - '10141'
short-url:
    - 'http://bit.ly/n7hkyf'
categories:
    - Embedded
tags:
    - RNS510
---

![](http://t2.gstatic.com/images?q=tbn:ANd9GcRI2QmWOaex_r7-CT5HVByOsEOLkoPkJObabRnm1oNW7YHijsqB)If you have read my earlier blogs about the RNS 510 then you may know I have been working on creating custom startup logo’s for some time.

For some time now I know how the images are encoded and decoded between Windows bitmap and the native RNS 510 image format.

Recently I identified the checksums in the image data, and the differing parameters for the Seat, Skoda and VW versions of the RNS 510 firmware.

But yesterday I noticed that there’s now an online tool that can create an iso image with a custom startup logo.

Nice? Well I am not sure!

First of all the person who wrote this online tool seems to have good intentions and he wants his tool to be freely available.

However he just reversed the ISO images that float around the net. Note that I did NOT create those images, they are most likely created with Josi’s [SetConfig](http://vwnavi.com/showthread.php/14638-display-backlight-not-working-after-upgrade-to-3810-3970) tool:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/09/image_thumb24.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/09/image24.png)

He calls the Author from this tool, a guy that has done very much for the RNS 510 community, an [Idiot](http://www.skodaforum.nl/board/showthread.php?16501-Boot-logo-file-formaat-reverse-engineering).

But back to the site, it is published [here](http://www.netdata.be/iso/) and this is a screenshot:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/09/image_thumb25.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/09/image25.png)

My quick observations from the tool:

- Seat option is missing (I guess the author didn’t find any Seat ISO’s on the net). I can say to him that the correct ID for Seat is **04 16 75** and the Index is always 0.
- Checksums inside logo.fli are correct.
- The author doesn’t seem to fully understand the checksum mechanism in version.txt yet but it’s not a problem here because the iso has only a few files.
- If you have a VW RNS510 with DynAudio then his tool only seems to replace the regular logo and not the DynAudio logo.
- This tool doesn’t seem to have an option for the Chinese version of the RNS 510.
- I did a test with a logo already present on the site using my test tool and it seems that the conversion looses some details:

<font color="#4c4c4c">Original from site:</font>

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/09/image_thumb26.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/09/image26.png)

As seen in the Logo.fli:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/09/image_thumb27.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/09/image27.png)

Just my 5 cents.

Related articles I wrote earlier:

- [RNS 510 Checksum Calculator](http://192.168.40.25:8081/2011/05/20/rns-510-checksum-calculator/)
- [VW RNS 510 Navigation Startup Pictures](http://192.168.40.25:8081/2011/05/04/vw-rns-510-navigation-startup-pictures/)
- [Changing the RNS 510 startup logo](http://192.168.40.25:8081/2011/08/31/changing-the-rns-510-startup-logo/)
- [RNS510 firmware has new startup logo’s](http://192.168.40.25:8081/2011/09/02/rns510-firmware-has-new-startup-logos/)
- [RNS 510 Editor](http://192.168.40.25:8081/2011/09/20/rns-510-editor/)