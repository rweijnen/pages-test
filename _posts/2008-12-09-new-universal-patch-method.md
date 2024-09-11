---
id: 211
title: 'New Universal Patch Method'
date: '2008-12-09T12:50:23+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2008/12/09/new-universal-patch-method/'
permalink: /2008/12/09/new-universal-patch-method/
views:
    - '30545'
short-url:
    - 'http://bit.ly/euUIhW'
categories:
    - General
---

Up until now I used [VPatch](http://www.tibed.net/vpatch/) for all my patches. The disadvantage of VPatch is that it uses strict MD5 hash checks. This means that a patch can only be applied to **exactly** the same file the patch was based on (exact same build and language).

Because many people are asking for patches for other builds and languages I decided to move over to another patch mechanism. This will use search &amp; replace on specific Hex bytes.

The consequence is that patching another build or language version is possible. However there is no absolute certaintity that the patch will work on other builds or languages. Ofcourse the patcher will only patch if the specific bytes were found which is safer than patching an offset.

It’s up to the user to carefully test the patched file and hopefully report back to me if it’s working.

Now I will describe how to use this universal patcher.

First step is you need to download the tool that “compiles” the patch from an external site (link is below the article) called DUP2. Extract the archive content to a folder on your drive (I will assume it to be C:\\Program Files\\DUP2). Please note that I am not the author of this tool nor do I even know the author.

Next step is you download the .dup2 project file for the patch you’d like to use. Place this in the Projects subfolder of DUP2.

Now doubleclick the downloaded .dup2 file and click on the “Create Patch” button:

[![dup2](http://192.168.40.25:8081/wp-content/uploads/2008/12/dup2-small.png)](http://192.168.40.25:8081/wp-content/uploads/2008/12/dup2.png)

Now you will be asked to save the patch.exe file somewhere:

[![dup2save](http://192.168.40.25:8081/wp-content/uploads/2008/12/dup2save-small.png)](http://192.168.40.25:8081/wp-content/uploads/2008/12/dup2save.png)

Close DUP2 and start the saved exe file (make sure the file you want to patch is in the same directory as the patch.exe), this will show you a screen similar to the screenshot below:

![dup2patch](http://192.168.40.25:8081/wp-content/uploads/2008/12/dup2patch.png)

Click on the Patch button to patch the file, a backup with the added .BAK extension will automatically be created.

![dup2patchdone](http://192.168.40.25:8081/wp-content/uploads/2008/12/dup2patchdone.png)

That’s all!

**Please do not distribute patch.exe files, they are for personal use only.**

DUP2 Link: [here](http://diablo2oo2.di.funpic.de/downloads/dup2.rar)