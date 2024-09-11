---
id: 1209
title: 'The case of the Annotations Toolbar'
date: '2011-01-17T22:24:22+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1209'
permalink: /2011/01/17/the-case-of-the-annotations-toolbar/
views:
    - '1566'
short-url:
    - 'http://bit.ly/go9EqZ'
categories:
    - General
    - 'Windows 2003'
---

I got some interesting questions from a user today regarding TIFF images on a Windows 2003 based Citrix environment.

This user has an application that works with scanned documents and for each document exists both a pdf and a tiff version in the application.

By default the TIF (and TIFF) file extensions are linked to the Windows Picture and Fax Viewer in Windows 2003.

The user told me that some time ago she had an extra toolbar where she could perform some extra operations such as making a selection on TIFF images.

At some point in time this mysterious Toolbar disappeared and she was never able to get it back. She reported this to the helpdesk and the system administrator but they were unable to resolve this.

I hadn’t hear of this toolbar before but a Google Search led me to [this page](http://www.microsoft.com/resources/documentation/windows/xp/all/proddocs/en-us/image_toolbar.mspx?mfr=true) which explains the toolbar in question which is called the *Annotation Toolbar*.

I opened the same TIFF image and noticed the toolbar wasn’t there:

[![clip_image002](http://192.168.40.25:8081/wp-content/uploads/2011/01/clip_image002_thumb.jpg "clip_image002")](http://192.168.40.25:8081/wp-content/uploads/2011/01/clip_image002.jpg)

But I also noticed that the file was readonly, so I removed the readonly attribute and openend it again and there was the Toolbar:

[![clip_image002[4]](http://192.168.40.25:8081/wp-content/uploads/2011/01/clip_image0024_thumb.jpg "clip_image002[4]")](http://192.168.40.25:8081/wp-content/uploads/2011/01/clip_image0024.jpg)

I wondered why the application was explicitly settings the images to readonly and I found a possible reason in a Microsoft KB Article: [TIFF documents are corrupted when you rotate them in Windows Picture and Fax Viewer](http://support.microsoft.com/kb/970922).

This article states: This problem does not occur if the TIFF document is set to read-only.

Sadly it doesn’t state: *but if you do that you loose the Annotations Toolbar*.