---
id: 1386
title: 'Move WSUS content folder'
date: '2011-02-02T11:15:55+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/02/02/move-wsus-content-folder/'
permalink: /2011/02/02/move-wsus-content-folder/
views:
    - '8162'
short-url:
    - 'http://bit.ly/eMpylz'
categories:
    - General
---

I needed to relocate the WSUS content folder because it was placed on the C Drive (even though there was a 2nd 150 GB Data Partition) to prevent WSUS from filling up the OS Drive.

Unfortunately there doesn’t seem to be a GUI option to do that, all Google searches lead to a GUI option for Small Business Server.

The [WSUSUtil](http://technet.microsoft.com/en-us/library/cc720466(WS.10).aspx) tool can do it however, which is located in %ProgramFiles%\\Update Services\\Tools by default.

You need to create the targetfolder and then issue:

 “&gt;

The tool will copy, and not move, the files to the new location and update the WSUS settings.

So don’t forget to manually delete the old folder!

The current storage location can be found or verified in the registry. It is stored in the *ContentDir* value in the *HKLM\\SOFTWARE\\Microsoft\\Update Services\\Server\\Setup* key.

See also [Verifying WSUS Server Settings](http://technet.microsoft.com/en-us/library/cc708545(WS.10).aspx) (although in incorrectly states the ContentDir value as Content).