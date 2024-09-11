---
id: 3308
title: 'ClickOnce Applications in Enterprise Environments'
date: '2013-08-05T16:51:21+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3308'
permalink: /2013/08/05/clickonce-applications-in-enterprise-environments/
short-url:
    - 'http://bit.ly/1c3amaR'
views:
    - '16754'
categories:
    - Citrix
    - 'Windows 2003'
    - 'Windows 2008'
    - 'Windows 2008 R2'
    - 'Windows 7'
    - 'Windows 8'
    - 'Windows XP'
tags:
    - ClickOnce
---

[ClickOnce](http://msdn.microsoft.com/en-us/library/t71a733d.aspx) is a Microsoft technology that enables an end user to install an application from the web without administrative permissions.

**<u>That’s great isn’t it?   
</u>**While ClickOnce may sound great to developers it’s actually a nightmare for Enterprise administrators because they try to prevent users from installing software themselves.

ClickOnce also incorporates an Automatic Updates mechanism which means that users might run different or not tested/approved versions…

**<u>Virtual Environments   
</u>**It get’s even worse in virtual environments such as VDI and SBC where machines are often non-persistent. Each time the users starts the application they will see a screen similar to the one below while they actually download and install it over and over again:

[![SNAGHTML87937a](http://192.168.40.25:8081/wp-content/uploads/2013/08/SNAGHTML87937a_thumb.png "SNAGHTML87937a")](http://192.168.40.25:8081/wp-content/uploads/2013/08/SNAGHTML87937a.png)

If the environment is persistent, it’s not guaranteed that the user works on the same machine each day. This means that the application will be installed on every box the user ever logs onto…

**<u>How does it work?   
</u>**In order to understand how we can best treat ClickOnce applications we need to understand how they work since MSDN documentation does not describe this in detail.

**<u>Install Location</u>**   
ClickOnce does this by installing all required files into   
%userprofile%\\Local Settings\\Apps\\2.0\\&lt;random folder&gt;\\&lt;random app id&gt;.

The &lt;random folder&gt; is composed out of the first 11 characters from the following registry key:

> HKCU\\Software\\Classes\\Software\\Microsoft\\Windows\\CurrentVersion\\Deployment\\SideBySide\\2.0\\ComponentStore\_RandomString

**<u>Assemblies and Components</u>**   
Since ClickOnce is .NET framework technology, it’s likely that such an application comes with .NET assemblies (dll’s).

.NET Assemblies are usually installed and registered in the Global Assembly Cache (GAC) but since that requires administrative permissions they are simply places in the application folder.

Therefore ClickOnce uses Registration-Free activation as described [here](http://msdn.microsoft.com/en-us/library/ms973915.aspx).

**<u>Launching a ClickOnce Application</u>**   
Finally a shortcut is created in the user part of the Start Menu to launch the application. The shortcut’s target points to an file with the extension "appref-ms". If we open this file with notepad, we can see that it contains the applications URL and the Application’s id:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/08/image_thumb.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/08/image.png)

appref-ms resolves to Application.Reference with the following Shell|Open action in the registry:

> rundll32.exe dfshim.dll,ShOpenVerbShortcut %1|%2

The actual process, as visible in TaskManager, that runs a ClickOnce application is dfsvc.exe.

**<u>So what to do with ClickOnce?</u>**   
How you deal with ClickOnce applications is up to you, let’s discuss a few options to handle them.

**<u>Option #1: Allow installation</u>**   
If it’s allright in your environment to have the user install ClickOnce applications it’s a lot friendlier to give them a start menu entry than a hyperlink. Use your favorite UEM or even a loginscript to create a start menu shortcut and point it to:

> rundll32.exe dfshim.dll,ShOpenVerbApplication http://ClickOnceUrl.application

Where the url matches what’s inside the appref-ms file and must end with .application

**<u>Option #2: Virtualize</u>**   
My personal preference would be to virtualize a ClickOnce applications using tools like App-V or ThinApp. This enabled us to deploy a ClickOnce application like all other applications and does not rely on the user to install them.

ClickOnce applications are hard to virtualize though. My first attempt was to use the normal capture process. But due to the launch process of ClickOnce application (through the Shell/Explorer.exe) you "step out the bubble" and therefore fail to load assemblies located inside the bubble. In my case the app just crashed but Event ID 59 was in the Event Log:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/08/image_thumb1.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/08/image1.png)

I troubleshooted this for a while and in the end the solution was so simple that it’s almost too easy!

You can simply pickup the whole application folder including all DLL’s and files, place that in the package and launch the exe from the folder…

To easily determine where the necessary files are you can used the mageui.exe tool from the platform sdk (I have attached this at the end of this post for convenience). Open the application’s manifest (usually named Appname.exe.manifest) and click the Files Tab:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/08/image_thumb2.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/08/image2.png)

So collect those files, place them in a (virtual) folder of choice and use the exe listed as Entry Point. That’s it!

**<u>Option #3: Use a fileshare</u>**   
Using the same method as option #2, just place the files on a network share and launch the files form there 😉

**<u>Disable Auto-Update</u>**   
I have not (yet) found a way to block updates on a ClickOnce application. If you do, please let me know so I can add it to this post!

 [ mage (4199 downloads ) ](http://192.168.40.25:8081/download/mage/?tmstv=1726048920 "Version platform sdk v6.0a")