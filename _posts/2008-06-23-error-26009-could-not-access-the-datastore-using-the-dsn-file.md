---
id: 105
title: 'Unattended Citrix Installation: Could not Access the datastore using the DSN file'
date: '2008-06-23T22:11:21+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2008/06/23/error-26009-could-not-access-the-datastore-using-the-dsn-file/'
permalink: /2008/06/23/error-26009-could-not-access-the-datastore-using-the-dsn-file/
views:
    - '5697'
short-url:
    - 'http://bit.ly/gUwHVk'
categories:
    - Citrix
    - script
tags:
    - Citrix
    - datastore
    - dsn
    - error
    - 'error 26009'
    - Installation
    - Unattended
---

I was working on an unattended installation of Citrix Presentation Server 4.5 or rather Citrix Xenapp. I was creating the dsn file for the installation by a script that uses the echo command and output this to a file.

This is a part of the script:

> rem Create ODBC file  
> rem ———————————————————————–  
> echo \[ODBC\] &gt; %ODBC%  
> echo DRIVER=SQL SERVER &gt;&gt; %ODBC%  
> echo UID=%SQL\_SA% &gt;&gt; %ODBC%  
> echo Address=%SQL\_SERVER%,1433 &gt;&gt; %ODBC%  
> echo Network=DBMSSOCN &gt;&gt; %ODBC%  
> echo LANGUAGE=us\_english &gt;&gt; %ODBC%  
> echo DATABASE=%CTX\_DATASTORENAME% &gt;&gt; %ODBC%  
> rem echo WSID=%COMPUTERNAME% &gt;&gt; %ODBC%  
> echo APP=Citrix IMA &gt;&gt; %ODBC%  
> echo SERVER=%SQL\_SERVER% &gt;&gt; %ODBC%  
> echo Description=Citrix Datastore &gt;&gt; %ODBC%  
> echo. &gt;&gt; %ODBC%

Even though the generated DSN file looks ok the installation fails. If you look in the installation log you can see this error: Error 26009. Could not Access the datastore using the DSN file.

I then created a dsn file through the ODBC Data Source Administrator and then the installation went ok. I compared the DSN file with the one my script generated and it was the same.

A search with [Google](http://www.google.nl/search?sourceid=navclient&aq=t&ie=UTF-8&rlz=1T4ADBR_enNL263NL264&q=Error+26009%2e+Could+not+Access+the+datastore+using+the+DSN+file "Error 26009. Could not Access the datastore using the DSN file") and in the [Citrix forums](http://support.citrix.com/search/forum/?searchQuery=%22Error+26009.+Could+not+Access+the+datastore+using+the+DSN+file%22&categoryId=&modifiedWhen= "Results for http://support.citrix.com/search/forum/?searchQuery=%22Error+26009.+Could+not+Access+the+datastore+using+the+DSN+file%22&categoryId=&modifiedWhen=") leads to numerous posts with the same error but none with a real solution. Some suggestions are that you need to remove the WSID line or even the order of the entries in the DSN file. But none of these suggestions work.

So I compared the two files again and I noticed that the filesize of my generated DSN was slightly bigger. So let’s look again at the script:

> echo DRIVER=SQL SERVER &gt;&gt; %ODBC%

See the space right before the &gt;&gt;? This means that after each line in the dsn file there’s a space too. If you open the file with a Hex Viewer you can easily see the spaces (ASCII value 20):

![hexdump](http://192.168.40.25:8081/wp-content/uploads/2008/06/hexdump.png)

So the solution is to change this (for all lines) to:

> echo DRIVER=SQL SERVER&gt;&gt; %ODBC%

After that it works perfectly!