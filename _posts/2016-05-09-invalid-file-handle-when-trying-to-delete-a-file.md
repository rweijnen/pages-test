---
id: 3668
title: 'Invalid file handle when trying to delete a file'
date: '2016-05-09T12:30:02+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3668'
permalink: /2016/05/09/invalid-file-handle-when-trying-to-delete-a-file/
views:
    - '3277'
short-url:
    - 'http://bit.ly/1WiD2ml'
    - 'http://bit.ly/1WiD2ml'
categories:
    - 'Windows Internals'
---

When I was trying to delete a folder from my local harddrive (cygwin64 in my case) I got the following error message: “*Invalid file handle.*“:

[![Invalid file handle](http://192.168.40.25:8081/wp-content/uploads/2016/05/image_thumb.png "1 Interrupted Action")](http://192.168.40.25:8081/wp-content/uploads/2016/05/image.png)

I then attempted to delete the folder from the command prompt which failed as well with an “*Access is denied*” error:

[![image](http://192.168.40.25:8081/wp-content/uploads/2016/05/image_thumb-1.png "image")](http://192.168.40.25:8081/wp-content/uploads/2016/05/image-1.png)

When I tried to open the CON.cif file with notepad it showed “*Cannot find the file*” error:

[![Cannot find the file \\.\CON.txt file | Do you want to create a new file?](http://192.168.40.25:8081/wp-content/uploads/2016/05/image_thumb-2.png "Notepad")](http://192.168.40.25:8081/wp-content/uploads/2016/05/image-2.png)

The MSDN article “[Naming Files, Paths, and Namespaces](https://msdn.microsoft.com/en-us/library/aa365247(VS.85).aspx#maxpath)” explains why:

- Do not use the following reserved names for the name of a file:   
    CON, PRN, AUX, NUL, COM1, COM2, COM3, COM4, COM5, COM6, COM7, COM8, COM9, LPT1, LPT2, LPT3, LPT4, LPT5, LPT6, LPT7, LPT8, and LPT9. **Also avoid these names followed immediately by an extension**; for example, NUL.txt is not recommended
 The solution is to prefix the file name with either [\\\\.\\](file://\\.\) or [\\\\?\\](file://\\?\) and delete the file from the command prompt.