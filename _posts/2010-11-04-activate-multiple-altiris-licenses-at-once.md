---
id: 728
title: 'Activate Multiple Altiris Licenses at Once'
date: '2010-11-04T10:26:39+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=728'
permalink: /2010/11/04/activate-multiple-altiris-licenses-at-once/
views:
    - '8'
short-url:
    - 'http://bit.ly/edkbu1'
categories:
    - Altiris
---

I have had to activate Altiris Licenses a lot in the past and this is a task that could be done in a few minutes however in the case of using the HP branded version (HP Insight Server control, previously named HP Rapid Deployment Pack) it’s not.

This is because HP delivers a seperate license for each server which you have to load manually. An annoying thing if you have a lot of them.

I was already looking if I could automate this with some kind of script since I know from the past that the Product Licensing Utility embeds the license into the Express.exe file.

I decided to delete all existing licenses and then add one single license and compare the express.exe. My thought was that the license is simply added to the end of the exe file.

So I fired up the Product Licensing Utility, left the Activation Key File Information field blank and clicked Next.

[![Altiris License Activation Key Wizard](http://192.168.40.25:8081/wp-content/uploads/2010/11/altirislictool-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/11/altirislictool.png)

To my surprise a screen popped up where I was able to select multiple licenses (which is not possible in the previous screen):

[![ActivationKeyFiles](http://192.168.40.25:8081/wp-content/uploads/2010/11/activationkeyfiles-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/11/activationkeyfiles.png)

Then I clicked OK and yes, all licenses are loaded at once:

[![NodesSelected](http://192.168.40.25:8081/wp-content/uploads/2010/11/nodesselected-1-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/11/nodesselected-1.png)

I wish I had know this earlier…