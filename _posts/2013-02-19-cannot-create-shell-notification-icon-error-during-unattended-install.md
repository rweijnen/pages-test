---
id: 3034
title: 'Cannot create shell notification icon error during unattended install'
date: '2013-02-19T15:52:33+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3034'
permalink: /2013/02/19/cannot-create-shell-notification-icon-error-during-unattended-install/
views:
    - '1551'
short-url:
    - 'http://bit.ly/153aTTi'
categories:
    - 'Unattended Installation'
    - 'Windows XP'
tags:
    - InstallAware
    - Unattended
---

I was troubleshooting an unattended installation of a particular application. The install seemed to hang right away so I figured it was presenting some kind of message (error?).

Using a Window Spy tool I made the setup process visible and saw the following message:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/02/image_thumb.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/02/image.png)

The unattended install was supplied by the vendor and apparently they use InstallAware.

The setup tries to create a Tray Icon, probably a setup progress indicator, but this fails because there is no shell running (the installation is pushed from a deployment server).

The setup.exe extracts a bunch of files, including the actual installer executable and places this in a temp folder. Using Process Explorer I tracked down the path:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/02/image_thumb1.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/02/image1.png)

I opened the setup executable in Ida Pro and searched for the string "Cannot create shell notification icon" on the Strings window:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/02/image_thumb2.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/02/image2.png)

Then I checked where in the code this string is referenced (doubleclick on the string and press Ctrl-X):

[![SNAGHTML1b699626](http://192.168.40.25:8081/wp-content/uploads/2013/02/SNAGHTML1b699626_thumb.png "SNAGHTML1b699626")](http://192.168.40.25:8081/wp-content/uploads/2013/02/SNAGHTML1b699626.png)

From the Disassembly we can see that sub\_4C3F0C is called and if this returns a value &gt; 0 (Boolean TRUE) we jump to loc\_4C39F6. if the return value is 0 the error message is displayed:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/02/image_thumb3.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/02/image3.png)

sub\_4C4F0C makes a call into the [Shell\_NotifyIcon](http://msdn.microsoft.com/en-us/library/windows/desktop/bb762159%28v=vs.85%29.aspx) API:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/02/image_thumb4.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/02/image4.png)

I decided to patch the code by replacing the call to Shell\_NotifyIcon with "return TRUE". I have 6 bytes to do this:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/02/image_thumb5.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/02/image5.png)

To return TRUE we need to set the EAX CPU register (which holds the return value) to 1. If I would use mov eax, 0 this would take up 5 bytes. To get the same result in less bytes we can xor eax with itself (value becomes 0) and the increment it with 1.

Finally we return with retn 8 (8 because the function takes two arguments which are both 4 bytes in a 32 bit application):

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/02/image_thumb6.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/02/image6.png)

Now the installation continues without errors (screen belows shows the non silent installation):

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/02/image_thumb7.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/02/image7.png)