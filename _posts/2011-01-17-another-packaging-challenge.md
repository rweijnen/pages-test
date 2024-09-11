---
id: 1198
title: 'Another packaging challenge'
date: '2011-01-17T09:15:37+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1198'
permalink: /2011/01/17/another-packaging-challenge/
short-url:
    - 'http://bit.ly/gEVfRk'
views:
    - '2561'
categories:
    - 'Application Compatibility'
    - Java
    - Packaging
    - 'Unattended Installation'
tags:
    - InnoSetup
---

Another post on something that happened last week, this time it’s about a Java based Application again.

This particular application wanted to download three DLL’s from the Webserver to the Java bin directory.

This presents us with several issues on a multi user server such as a Citrix of Terminal Server:

1. <span style="color: #35383d;">The user does not have write permissions in this directory</span>
2. <span style="color: #35383d;">If we we give the user write permissions here what happens when the DLL’s are in use by another user?</span>

I assumed that if I preinstalled the DLL’s the application wouldn’t try to overwrite them but that didn’t work.

Then I monitored with Process Monitor if the Application wrote some kind of check file but at first I didn’t find anything.

So I decided to use the CorrectFilePaths shim to redirect the DLL’s to the user’s homedirectory (see [Using the CorrectFilePaths shim to redirect an ini file to a writable location](/blog/2010/12/28/using-the-correctfilepaths-shim-to-redirect-an-ini-file-to-a-writable-location/) for an explanation).

This worked nicely, I started the Application and saw the files being downloaded to the user’s homedirectory.

Side note: this downloading took a much too long time because it downloads byte by byte, #crappy!

I tested a few things in the application according to the instructions and everything seemed to work fine. Then I closed the Application and finally the Browser.

And then an error message came, indicating that the Application couldn’t a write a file called webutil.properties into the Java Home directory.

I then redirected this file as well and checked the content and it’s an ini alike file:  
“&gt;  
I then removed the CorrectFilePaths shim and place the webutil.properties file in the Java Directory and the DLL’s in the bin directory.

Now everything worked well but of course the challenge is how to package this.

The easiest approach is to have the installer copy the files but what happens if a webutil.properties file is already present?

I think it’s risky to blindly write this file so I wanted to an installer that does the following:

- <span style="color: #35383d;">Dynamically determine Java Folder </span>
- <span style="color: #35383d;">Create the webutil.properties file if it doesn’t exist</span>
- <span style="color: #35383d;">Add’s the DLL entries to the file</span>
- <span style="color: #35383d;">Does not overwrite existing entries</span>
- <span style="color: #35383d;">Does not write the lines if they already exist</span>
- On Uninstall remove the DLL files
- On Uninstall remove the lines from the webutil.properties file

I used [Innosetup](http://jrsoftware.org/isinfo.php) for this Task because it has a very nice scripting engine that makes this task a breeze. And best of all: Innosetup is free.

Below my Setup.iss script, I think the script parts contains enough comments to explain the code. But if you have any questions please leave a comment.

If you want to use the script, combine the setup and code part to one .iss file and compile it.  
The setup part:  
“&gt;  
And the script part:  
“&gt;