---
id: 2826
title: 'Replacing WFP Protected files'
date: '2012-12-05T15:33:45+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2826'
permalink: /2012/12/05/replacing-wfp-protected-files/
short-url:
    - 'http://bit.ly/UeFq8k'
views:
    - '1972'
categories:
    - 'Windows 2003'
    - 'Windows Internals'
    - 'Windows XP'
---

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb5.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image5.png)On Windows 2000, XP and Server 2003 a mechanism called [Windows File Protection](http://support.microsoft.com/kb/222193/en-us) (WFP) is used to protect system integrity.

How does WFP Work?   
Inside SFCFILES.DLL a list of files is kept that are monitored for changes. When a monitored file gets deleted, modified or overwritten WFP will restore the original from one of the following locations:

- <font color="#35383d">Cache Folder (%systemroot%\\System32\\DllCache)</font>
- <font color="#35383d">Network Installation Path</font>
- <font color="#35383d">Windows CD (or i386 folder on harddisk)</font>

But what if we need to replace such a file? You could write a batch file that copies the modified file to the cache folder, installation path and destination. And this may work if it’s quick enough.

A more reliable method is to use an undocumented export from sfc\_os.dll called SfcFileException (only exported by ordinal #5).

It’s signature is: DWORD WINAPI SfcFileException(RPC\_BINDING\_HANDLE hServer, LPCWSTR lpSrc, DWORD dwUnknown)

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb6.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image6.png)This function makes an RPC call to Winlogon and possibly we can even call this remotely. If we pass null for the server handle a Local RPC Connection will be setup:

 “&gt;

After calling the SfcFileException the given file is not monitored for a minute. After this minute it will be monitored again but only for new changes, the modified file that was places within the minute will not be restored.

This makes the call to this API very easy so I wrote a commandline tool that calls the SfcFileException.

The tool takes one parameter: the filename:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb7.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image7.png)

When the call was successful you have one minute to update the given file. After this minute WFP starts monitoring the file again.

[ WfpReplace.zip (1889 downloads ) ](http://192.168.40.25:8081/download/wfpreplace-zip/?tmstv=1726048920 "Version 1.0")