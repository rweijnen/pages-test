---
id: 477
title: 'Removing the Uninstall or change a program button from the Explorer Command Bar'
date: '2009-11-25T10:52:13+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=477'
permalink: /2009/11/25/removing-the-uninstall-or-change-a-program-button-from-the-explorer-command-bar/
views:
    - '18113'
short-url:
    - 'http://bit.ly/hWP2AY'
categories:
    - Citrix
    - 'Terminal Server'
    - Vista
    - 'Windows 2008'
    - 'Windows 7'
---

Windows Vista introduced the Command Bar in Explorer which is sometimes also referred to as the Folder Band or the Task Band. The Command Bar is of course also present in Windows 7 and Server 2008 (R2).

[![CommandBar](http://192.168.40.25:8081/wp-content/uploads/2009/11/commandbar-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/11/commandbar.png)

This Command Bar shows possible tasks or actions depending on the active folder. I wanted to remove the “Uninstall or change a program” (in Dutch this is called “Een programma verwijderen of wijzigen”) button from the Computer view:

[![CommandBarButton](http://192.168.40.25:8081/wp-content/uploads/2009/11/commandbarbutton-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/11/commandbarbutton.png)

I would have expected this setting to be in Group Policy in the User Section\\Administrative Templates\\Control Panel\\Add or Remove Programs folder but that doesn’t work. Instead you need to go to User Configuration\\Administrative Templates\\Control Panel\\Programs:

[![HidePrograms](http://192.168.40.25:8081/wp-content/uploads/2009/11/hideprograms-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/11/hideprograms.png)

Now the button is gone:

[![ButtonGone](http://192.168.40.25:8081/wp-content/uploads/2009/11/buttongone-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/11/buttongone.png)