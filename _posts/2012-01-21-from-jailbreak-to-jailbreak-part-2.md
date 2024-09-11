---
id: 2351
title: 'From Jailbreak to Jailbreak part 2'
date: '2012-01-21T14:02:15+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2012/01/21/from-jailbreak-to-jailbreak-part-2/'
permalink: /2012/01/21/from-jailbreak-to-jailbreak-part-2/
views:
    - '1291'
short-url:
    - 'http://bit.ly/ynpjYE'
categories:
    - iPhone
tags:
    - iOS
    - Jailbreak
---

[![image](http://www.peppercrew.nl/wp-content/uploads/2012/01/image_thumb19.png "image")](http://www.peppercrew.nl/wp-content/uploads/2012/01/image19.png)In this post, which is a followup on my [From JailBreak to Jailbreak](http://192.168.40.25:8081/2012/01/04/from-jailbreak-to-jailbreak/) post, I will describe the same procedure for A5 devices (iPhone 4S and iPad 2).

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/01/image_thumb15.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/01/image15.png)A lot of the stuff is really the same so I will not describe that again, this includes the actual update to iOs 5.01, xBackup, SHSH signatures and backup using iTunes.

Currently the Jailbreak for A5 devices with iOS 5 is only for iOS 5.01. Since Apple is expected to release iOS 5.1 very soon it’s highly recommended to **<span style="text-decoration: underline;">[update to iOS 5.01 NOW](http://www.peppercrew.nl/index.php/2012/01/update-your-a5-devies-to-ios-5-01-now/)</span>**. Especially because it’s not yet possible to downgrade to iOS 5.01 using Tiny Umbrella.

Although a Windows version of the Jailbreak will be released soon (in the form of an updated Redsn0w) I decided to do the Jailbreak now on my iPad2 3G.

**Update:** the Windows version of the Absinthe Jailbreak has been released and can be found [here](http://cache.greenpois0n.com/dl/absinthe-win-0.2.zip). Note that all steps below and screenshots refer to the Mac OS X version on a virtualized OS X.

**<span style="color: #ff0000;">Before you begin, delete any VPN Connections you might have defined on your device.</span>**

I used VMWare Workstation and a Virtual Machine with Mac OS X and it worked perfectly.

| Connect your iPad2 to the Virtual Machine: | [![SNAGHTML243c16](http://192.168.40.25:8081/wp-content/uploads/2012/01/SNAGHTML243c16_thumb.png "SNAGHTML243c16")](http://192.168.40.25:8081/wp-content/uploads/2012/01/SNAGHTML243c16.png) |
|---|---|
| Launch Absinthe and press the Jailbreak button: | [![image](http://192.168.40.25:8081/wp-content/uploads/2012/01/image_thumb16.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/01/image16.png) |
| The Jailbreak starts automatically, you do not need to go into DFU mode. Note that this first step may take a while (around 30 minutes in my case): | [![image](http://192.168.40.25:8081/wp-content/uploads/2012/01/image_thumb17.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/01/image17.png) |
| When the process finishes the device is rebooted: | [![image](http://192.168.40.25:8081/wp-content/uploads/2012/01/image_thumb18.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/01/image18.png) |
| Then the actual Payload is sent: | [![image](http://192.168.40.25:8081/wp-content/uploads/2012/01/image_thumb19.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/01/image19.png) |
| And if all is well you should end with this screen: | [![image](http://192.168.40.25:8081/wp-content/uploads/2012/01/image_thumb20.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/01/image20.png) |

If the above steps went well you should have a Green Pois0n icon, just click it and the rest of the process should perform automatically.

[![Cydia Icon](http://192.168.40.25:8081/wp-content/uploads/2012/01/image_thumb2.png "Cydia Icon")](http://192.168.40.25:8081/wp-content/uploads/2012/01/image2.png)In my case it didn’t but I could manually trigger it by going to Settings | VPN (there should be a Jailbreak VPN entry) and connect to it. My iPad automatically rebooted after which the Cydia icon appeared.

Last step is start Cydia which will adjust the filesystem and the Jailbreak is finished.

I restored my Cydia settings, repositories and apps using xBackup as I described in the [previous post](http://192.168.40.25:8081/2012/01/04/from-jailbreak-to-jailbreak/). The other steps like installing Installous and synching Apps with iTunes are identical as well.

Have fun and please let me know if you have tips or any steps I missed!