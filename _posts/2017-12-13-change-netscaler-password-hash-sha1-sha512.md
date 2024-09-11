---
id: 4167
title: 'Change NetScaler Password Hash from SHA1 to SHA512'
date: '2017-12-13T13:53:08+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://192.168.40.25:8081/?p=4167'
permalink: /2017/12/13/change-netscaler-password-hash-sha1-sha512/
views:
    - '724'
categories:
    - Citrix
    - NetScaler
tags:
    - Citrix
    - Hash
    - NetScaler
    - 'NetScaler Gateway'
    - Password
    - SHA1
    - SHA512
---

This morning I wanted to install the NetScaler patch for the TLS padding vulnerability and of course I made a backup before deploying it.

**Note**: If you haven’t installed this patch yet I would recommended to do so: see [CTX230238](https://support.citrix.com/article/CTX230238) and check out the [ROBOT attack -Return Of Bleichenbacher’s Oracle Threat](https://robotattack.org/) page to check which other products you may have that are vulnerable.

Upon checking the backups (I always download the backup and verify that the archive is intact) I noticed that one of my NetScaler’s uses SHA1 for the password hash whilst the other one uses SHA512:

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/12/image_thumb.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/12/image.png)

I thought that this was a little strange as both NetScaler’s are running the exact same build. However one of them (the one that uses SHA512) was reinstalled recently whilst the one using SHA1 has been upgraded.

This explains why there’s a difference. However since SHA1 (password) hashes are no longer considered secure I think it’s important to get rid of it and fortunately this is as simple as changing your password, something you (and I) should have done anyway!

From the GUI this can be can be done via System -&gt; User Administration

[![image](http://192.168.40.25:8081/wp-content/uploads/2017/12/image_thumb-1.png "image")](http://192.168.40.25:8081/wp-content/uploads/2017/12/image-1.png)

Or from the CLI using  
“&gt;