---
id: 1115
title: 'Fixing Applications: The Next Step'
date: '2011-01-07T14:39:55+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1115'
permalink: /2011/01/07/fixing-applications-the-next-step/
short-url:
    - 'http://bit.ly/fBFDMh'
views:
    - '2589'
categories:
    - 'Application Compatibility'
    - General
    - Packaging
    - 'Unattended Installation'
---

Lately I wrote a few articles about fixing bad applications using Compatibility Shims. If you didn’t read them yet, here are the links:

- <div>[Using the CorrectFilePaths shim to redirect an ini file to a writable location](http://192.168.40.25:8081/2010/12/28/using-the-correctfilepaths-shim-to-redirect-an-ini-file-to-a-writable-location/)</div>
- <div>[Using the CorrectFilePaths Shim with Visual Basic Applications](http://192.168.40.25:8081/2010/12/28/using-the-correctfilepaths-shim-with-visual-basic-applications/)</div>
- <div>[Redirect Global Object to Local Objects (aka fix that bad app!)](http://192.168.40.25:8081/2011/01/06/redirect-global-object-to-local-objects-aka-fix-that-bad-app/)</div>

I also described that you can install an Application Compatibility Database using the sdbinst command.

At first I just took a script task in my Altiris Server to deploy the database using sdbinst -q &lt;dbname&gt; but later on I got a better idea.

I decided to build a seperate installer for the Compatibility Database that will upon install launch sdbinst -q and upon uninstall sdbinst -q -u.

I see several advantages to this approach:

- <div>You can check/see in Add/Remove Programs that a Compatibility Fix for a certain application has been installed.</div>
- <div>You can easily Uninstall a fix when it’s no longer required (eg when the Vendor has updated the Application)</div>

[![AddRemovePrograms](http://192.168.40.25:8081/wp-content/uploads/2011/01/addremoveprograms-small2.png)](http://192.168.40.25:8081/wp-content/uploads/2011/01/addremoveprograms.png)

You can of course use your utility of choice to create an Installer, below a sample using a free tool called InnoSetup.

First go to the [InnoSetup](http://www.jrsoftware.org/isinfo.php) site and download the QuickStart Pack, this includes InnoIde which is a GUI front-end for creating and editing Inno Setup Scripts.

Use the Wizard button to start the Setup Script Wizard and Click Next

[![Inno1](http://192.168.40.25:8081/wp-content/uploads/2011/01/inno1-small2.png)](http://192.168.40.25:8081/wp-content/uploads/2011/01/inno1.png)

Fill in Application Information for the Fix and Click Next:

[![Inno2](http://192.168.40.25:8081/wp-content/uploads/2011/01/inno2-small2.png)](http://192.168.40.25:8081/wp-content/uploads/2011/01/inno2.png)

This is a very simple sample, so we don’t need an Application Folder. You may wish to create one and copy the database into that folder.

[![Inno3](http://192.168.40.25:8081/wp-content/uploads/2011/01/inno3-small2.png)](http://192.168.40.25:8081/wp-content/uploads/2011/01/inno3.png)

Deselect the #define options:

[![Inno4](http://192.168.40.25:8081/wp-content/uploads/2011/01/inno4-small1.png)](http://192.168.40.25:8081/wp-content/uploads/2011/01/inno4.png)

Click Next until you can Finish, now you have created a Project.

Go to Install Actions and select New Install Run Action:

[![AddInstallRun](http://192.168.40.25:8081/wp-content/uploads/2011/01/addinstallrun-small1.png)](http://192.168.40.25:8081/wp-content/uploads/2011/01/addinstallrun.png)

And add an Uninstall Action and Select New Run Uninstall Action:

[![AddUninstallRun](http://192.168.40.25:8081/wp-content/uploads/2011/01/adduninstallrun-small1.png)](http://192.168.40.25:8081/wp-content/uploads/2011/01/adduninstallrun.png)

Now use the Compile button to create a setup.exe (in the Output subfolder), use Setup.exe /SILENT to do an UnAttended install.