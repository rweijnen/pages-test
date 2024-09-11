---
id: 1083
title: 'Redirect Global Object to Local Objects (aka fix that bad app!)'
date: '2011-01-06T14:34:11+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1083'
permalink: /2011/01/06/redirect-global-object-to-local-objects-aka-fix-that-bad-app/
views:
    - '7150'
short-url:
    - 'http://bit.ly/gY8eqH'
categories:
    - 'Application Compatibility'
    - Oracle
    - Packaging
    - 'Unattended Installation'
---

Today I was working on Unattended Installation of an Application called SmartDocuments.

The application seemed to behave nicely on a Multi User (Citrix/Terminal Server) environment: it writes user configuration to the user part of the registry and writes configuration files in a user accessible path.

When testing with a normal user account I ran into a problem though, I got an Oracle ORA-01019 error message:

[![ORA01019](http://192.168.40.25:8081/wp-content/uploads/2011/01/ora01019-small.png)](http://192.168.40.25:8081/wp-content/uploads/2011/01/ora01019.png)

The English message is ORA01019: unable to allocate memory in the user side.

I suspected this was a permission issue so I ran a trace with Process Monitor but I was unable to find a resolution.

Therefore I assumed this application (or the Oracle client) tried to create an object in the Global namespace because this is a very common issue in Citrix/Terminal Server environments.

See the post [Accessing kernel objects in other sessions](http://192.168.40.25:8081/2009/01/27/accessing-kernel-objects-in-other-sessions/ "Accessing kernel objects in other sessions part 1") if you want to learn more about the Object namespace.

My next step was to use the WinObj tool from Sysinternals and I was quickly able to identify the suspect:

[![BNO](http://192.168.40.25:8081/wp-content/uploads/2011/01/bno-small.png)](http://192.168.40.25:8081/wp-content/uploads/2011/01/bno.png)

I assume the Oracle client is responsible for this and not the actual application but it’s a typical bad design.

Instead of referencing the \\Global prefix this application should have used the \\Local prefix.

The easiest solution is to give all users the [Create Global Objects Privilege](http://technet.microsoft.com/en-us/library/cc740217(WS.10).aspx) but this affects security so I think it’s the least preferred option.

A better solution is to use the Application Compatibilty Toolkit to redirect \\Global to \\Local using the [LocalMappedObject Fix](http://technet.microsoft.com/en-us/library/cc749287(WS.10).aspx "Using the LocalMappedObject Fix").

If you are not familiar with the Application Compability Toolkit please read my earlier post [Using the CorrectFilePaths shim to redirect an ini file to a writable location](http://192.168.40.25:8081/2010/12/28/using-the-correctfilepaths-shim-to-redirect-an-ini-file-to-a-writable-location/ "Using the CorrectFilePaths shim to redirect an ini file to a writable location") and [Using the CorrectFilePaths Shim with Visual Basic Applications](http://192.168.40.25:8081/2010/12/28/using-the-correctfilepaths-shim-with-visual-basic-applications/ "Using the CorrectFilePaths Shim with Visual Basic Applications").

So let’s apply the fix!

Open the Compatibilty Administrator (32 bit), create a new database and add a new Fix. Browse to the Program File Location of the Executable that accesses the database (in my case ConnectionManager.exe):

[![EditAppFix](http://192.168.40.25:8081/wp-content/uploads/2011/01/editappfix-small.png)](http://192.168.40.25:8081/wp-content/uploads/2011/01/editappfix.png)

Press Next and press Next again and in the Compatibility Fixes screen select LocalMappedObject and click on the Parameters button:

[![EditAppFix2](http://192.168.40.25:8081/wp-content/uploads/2011/01/editappfix2-small.png)](http://192.168.40.25:8081/wp-content/uploads/2011/01/editappfix2.png)

In the Command line edit enter the parameters:

![LocalMappedObjectOptions](http://192.168.40.25:8081/wp-content/uploads/2011/01/localmappedobjectoptions.png)

I used:

> <span style="text-decoration: line-through;">“\\Global\\D:/Apps/ORACLE/PRODUCT/10.2.0/CLIENT\_1/ORACORE/ZONEINFO/TIMEZLRG.DAT;\\Local\\D:/Apps/ORACLE/PRODUCT/10.2.0/CLIENT\_1/ORACORE/ZONEINFO/TIMEZLRG.DAT”</span>

<span style="color: #ff0000;">**EDIT: The LocalMappedObject shim simply redirects ALL Global objects to Local (thanks for this info to Chris Jackson) so the Parameter is not needed at all!**</span>

Click Finish, save the Database and Install it (read the previous articles for unattended installation of the database).

And not it works nicely:

![Working](http://192.168.40.25:8081/wp-content/uploads/2011/01/working.png)

Note: if the application consists of multiple executables that access the Object you must apply a fix for all of them.

I hope you enjoyed reading this article and found it useful, please leave a comment or rating!