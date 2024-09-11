---
id: 1030
title: 'Using the CorrectFilePaths shim to redirect an ini file to a writable location'
date: '2010-12-28T13:52:01+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1030'
permalink: /2010/12/28/using-the-correctfilepaths-shim-to-redirect-an-ini-file-to-a-writable-location/
views:
    - '7699'
short-url:
    - 'http://bit.ly/fANCVj'
categories:
    - 'Application Compatibility'
    - Packaging
    - 'Unattended Installation'
---

Today I needed to package an application called PlesirReality and I noticed that it wrote an ini file into the program directory (in my case D:\\Apps\\PlesirReality).

I looked into this ini file (areastate.ini) and it writes user settings in there, like the last position of the window etc.

We can see this easily with Process Monitor:

[![PlesirMon1](http://192.168.40.25:8081/wp-content/uploads/2010/12/usersrweijnenappdataroamingblogdeskuserdataimagesplesirmon1-small7.png)](http://192.168.40.25:8081/wp-content/uploads/2010/12/usersrweijnenappdataroamingblogdeskuserdataimagesplesirmon11.png)

This presents us with several problems:

The first problem is that the user does not have write permission in this directory so upon application exit we get an ugly error message:

![PlesirUglyError](http://192.168.40.25:8081/wp-content/uploads/2010/12/usersrweijnenappdataroamingblogdeskuserdataimagesplesiruglyerror5.png)

We could “fix” this by giving the user write permissions to this directory or even to this specific file but here comes the seconds problem:

On a multi user environment like Citrix or Terminal Server/Remote Desktop Services it means that when one user exists he will he save his settings for all users.

And then we have a third problem:

When a users starts the application on another server he will not get the expected settings because each server will have it’s own copy of this ini file.

Luckily Microsoft has a solution for us and it’s called the [Application Compatiblity Toolkit](http://www.microsoft.com/downloads/en/details.aspx?FamilyId=24DA89E9-B581-47B0-B45E-492DD6DA2971&displaylang=en "Microsoft Application Compatibility Toolkit 5.6"). I learned about it on the Teched Emea in 2008 in an excellent session from [Chris Jackson](http://blogs.msdn.com/b/cjacks/ "Chris Jackson's Semantic Consonance").

So let’s fix it!

On a test environment start the Compatibility Administrator (32-bit):

[![CompatAdm](http://192.168.40.25:8081/wp-content/uploads/2010/12/usersrweijnenappdataroamingblogdeskuserdataimagescompatadm-small5.png)](http://192.168.40.25:8081/wp-content/uploads/2010/12/usersrweijnenappdataroamingblogdeskuserdataimagescompatadm.png)

Create a new Database and save it (I named it PleasirRealtiy) and then click the Fix button. Use the Browse to point the to the executable and give it a meaningfull name:

[![PlesirFix1](http://192.168.40.25:8081/wp-content/uploads/2010/12/usersrweijnenappdataroamingblogdeskuserdataimagesplesirfix1-small5.png)](http://192.168.40.25:8081/wp-content/uploads/2010/12/usersrweijnenappdataroamingblogdeskuserdataimagesplesirfix1.png)

Click Next in the following Screen:

[![PlesirFix2](http://192.168.40.25:8081/wp-content/uploads/2010/12/usersrweijnenappdataroamingblogdeskuserdataimagesplesirfix2-small4.png)](http://192.168.40.25:8081/wp-content/uploads/2010/12/usersrweijnenappdataroamingblogdeskuserdataimagesplesirfix2.png)

Now select the CorrectFilePaths shim and click Parameters:

[![PlesirFix3](http://192.168.40.25:8081/wp-content/uploads/2010/12/usersrweijnenappdataroamingblogdeskuserdataimagesplesirfix3-small4.png)](http://192.168.40.25:8081/wp-content/uploads/2010/12/usersrweijnenappdataroamingblogdeskuserdataimagesplesirfix3.png)

The Command line should be “Original filename;New filename”, in my case I am redirecting the file to the user’s homedirectory (H:) in the Windows subdir:

![PlesirFixParams](http://192.168.40.25:8081/wp-content/uploads/2010/12/usersrweijnenappdataroamingblogdeskuserdataimagesplesirfixparams3.png)

Now Click OK, followed by Next. In the Matching Information screen you can limit the Fix to properties such as the version of the executable. This is usefull to prevent that you will apply the fix to future versions. But for now we will not use this so just click Finish:

[![PlesirFix4](http://192.168.40.25:8081/wp-content/uploads/2010/12/usersrweijnenappdataroamingblogdeskuserdataimagesplesirfix4-small3.png)](http://192.168.40.25:8081/wp-content/uploads/2010/12/usersrweijnenappdataroamingblogdeskuserdataimagesplesirfix4.png)

Now you need to save the Database and Install it:

![PlesirFixInstall](http://192.168.40.25:8081/wp-content/uploads/2010/12/usersrweijnenappdataroamingblogdeskuserdataimagesplesirfixinstall3.png)

And we can use Process Monitor to verify the results:

[![PlesirMon2](http://192.168.40.25:8081/wp-content/uploads/2010/12/usersrweijnenappdataroamingblogdeskuserdataimagesplesirmon2-small3.png)](http://192.168.40.25:8081/wp-content/uploads/2010/12/usersrweijnenappdataroamingblogdeskuserdataimagesplesirmon2.png)

As you can see the Application tries to open D:\\Apps\\PlesirReality but is redirector to H:\\Windows\\areastate.ini.

There is one final step: we need to install the database on all of our systems, this can be done using the sdbinst.exe tool that comes with windows (%systemroot%\\system32\\sdbinst.exe).

SdbInst /? shows the cmdline switches:

![sdbinst](http://192.168.40.25:8081/wp-content/uploads/2010/12/usersrweijnenappdataroamingblogdeskuserdataimagessdbinst2.png)

So we can install using sdbinst &lt;databasename&gt;:

[![sbdinst2](http://192.168.40.25:8081/wp-content/uploads/2010/12/usersrweijnenappdataroamingblogdeskuserdataimagessbdinst2-small2.png)](http://192.168.40.25:8081/wp-content/uploads/2010/12/usersrweijnenappdataroamingblogdeskuserdataimagessbdinst2.png)

Or quietly using -q: sdbinst -q PlesirReality.sdb

If you would like to learn more about using Shims to fix applications then see: [TechNet Virtual Lab: Mitigating Application Issues Using Shims](https://www.microsoft.com/resources/virtuallabs/step1-technet.aspx?LabId=dab9cb6c-6ba5-4f2e-bd38-f61c72fa9a09&BToken=reg)