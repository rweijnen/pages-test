---
id: 32
title: TSAdminEx
date: '2007-10-23T19:48:50+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2007/10/23/tsadminex/'
permalink: /2007/10/23/tsadminex/
views:
    - '7843'
short-url:
    - 'http://bit.ly/dO5nSR'
categories:
    - Delphi
    - Programming
    - 'Terminal Server'
tags:
    - 'Terminal Server'
    - TSAdmin
    - TSAdminEx
---

You will probably know Microsofts Tool to Manage Terminal Server, it’s called Terminal Services Manager (you will probably know it as TSAdmin). It can be used to view information about terminal servers including all sessions, users, and processes for each terminal server.

[![TSAdmin Screenshot](http://192.168.40.25:8081/wp-content/uploads/2007/10/tsadmin.thumbnail1.png)](http://192.168.40.25:8081/wp-content/uploads/2007/10/tsadmin1.png "TSAdmin Screenshot")

I’m currently working on a TSAdmin replacement (codename TSAdminEx). Purpose is to show how to use the Terminal Server API’s and as a little bonus we will add some extra functionality to TSAdminEx.

  
Well 1st step is to create the GUI and make it resemble TSAdmin, so we create a new Application and we place a Mainmenu, Toolbar (set Edgeborders to ebTop for the horizontal line) and Actionlist on it.   
Then we add a Treeview (set it’s Align property to alLeft) and a Splitter. Next add a bevel and a Panel, set Align to alClient and BevelOuter to bvLowered. Add a PageControl (align alClient) with 3 Tabsheets to the Panel and place a ListView on each Tabsheet.

[![TSAdminEx Screenshot](http://192.168.40.25:8081/wp-content/uploads/2007/10/tsadminex.thumbnail1.png)](http://192.168.40.25:8081/2007/10/23/tsadminex/tsadminex-screenshot/ "TSAdminEx Screenshot")

As you can see on the screenshot it’s already starting to look like TSAdmin after adding the Menus and Toolbar buttons.

So now we need to add some code but wait… now is your chance to add **your** functionality! Let me know by adding a comment what features you would like to see in TSAdminEx. Below what I’m currently thinking of:

- Launch a process in a Terminal Server session (see also: [http://192.168.40.25:8081/2007/10/20/how-to-launch-a-process-in-a-terminal-session/](http://192.168.40.25:8081/2007/10/20/how-to-launch-a-process-in-a-terminal-session/ "How to Launch a process in a Terminal Session")).
- Start an RDP session to a server by right clicking in the TreeView.
- Add domain to the user’s listview (handy for Multidomain environment).
- Requests?