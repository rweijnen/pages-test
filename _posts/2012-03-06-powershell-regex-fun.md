---
id: 2493
title: 'PowerShell RegEx Fun'
date: '2012-03-06T11:19:53+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2493'
permalink: /2012/03/06/powershell-regex-fun/
short-url:
    - 'http://bit.ly/wJrLyQ'
views:
    - '1658'
categories:
    - PowerShell
tags:
    - PowerShell
    - RegEx
---

I am writing a script that is going to automate a number of manual steps involved in creating a new image with Citrix PVS.

First step is to copy the most recent base image which is kept in a folder structure. The folder name is always YYYY-MM-DD (description):

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/03/image_thumb1.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/03/image1.png)

I needed to determine the most recent folder and didn’t want to rely on creation date. Instead I walk the directory tree and filter the date out of the filename with a regular expression:

“&gt;

Output:

“&gt;

Next step is to copy the base image to the PVS Folder and name it according to the following convention: XenApp5\_v{0}\_Off2k3\_Core where {0} is a 3 digit version number.

So we walk the PVS directory use a regular expression to filter out the version digits. We collect the version digits in an array and use the Measure-Object to obtain the highest number which we then increment by 1:

“&gt;

Output:

“&gt;

Last step for this post is to copy the image over to the PVS folder but since the files are large I wanted to have progress indication (which the Copy-Item cmdlet doesn’t have). I found a function that does exactly this on [stackoverflow](http://stackoverflow.com/questions/2434133/progress-during-large-file-copy-copy-item-write-progress), I only changed \[int\] to \[uint64\] to accommodate for files &gt; 2 GB:

“&gt;