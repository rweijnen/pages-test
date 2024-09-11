---
id: 368
title: 'Delegated Management Console'
date: '2009-06-12T21:40:20+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2009/06/12/delegated-management-console/'
permalink: /2009/06/12/delegated-management-console/
views:
    - '2020'
short-url:
    - 'http://bit.ly/fknCCx'
categories:
    - Citrix
    - 'Terminal Server'
---

In this topic I just want to show(case) you something I created in the past. It is a management console that enables delegated management in a Terminal Server or Citrix environment.

The console is launched by a small executable that check credentials (based on group membership) and then launches an RDP session with the actual console in it. The logic behind it is that the RDP session runs with an account with delegated permissions in Active Directory and the actual user account that logs in here doesn’t have any permissions at all.

This is the login screen:

[![login](http://192.168.40.25:8081/wp-content/uploads/2009/06/login-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/06/login.png)

If you’ve passed the login screen you enter the Main Console which consists of a Treeview on the left with possible options and a work area on the right:

![mainscreen](http://192.168.40.25:8081/wp-content/uploads/2009/06/mainscreen.png)

When you expand the first item (Active Directory) you get an Active Directory View just like the Active Directory &amp; Computers MMC (ADUC)snapin:

![adtree](http://192.168.40.25:8081/wp-content/uploads/2009/06/adtree.png)

Doubleclicking an Active Directory object shows the object’s properties, just like in ADUC:

![adprops](http://192.168.40.25:8081/wp-content/uploads/2009/06/adprops.png)

Of course you can change all properties in all tabs and besides adjusting properties we can also add a new user object through a wizard.

The Wizard takes care of input and decides the username based on the username convention, and you can add the user to a specific OU (Locatie):

![createuser1](http://192.168.40.25:8081/wp-content/uploads/2009/06/createuser1.png)

Next we fill in the mailbox properties, based on the OU the wizard automatically suggests the primary and secondary e-mail addresses:

![createuser2](http://192.168.40.25:8081/wp-content/uploads/2009/06/createuser2.png)

Optionally we can enter details like office, phone, fax and so on:

![createuser3](http://192.168.40.25:8081/wp-content/uploads/2009/06/createuser3.png)

Optionally we can copy settings from pre-defined template users, so our new users is put into the right groups and gets the proper permissions:

![createuser4](http://192.168.40.25:8081/wp-content/uploads/2009/06/createuser4.png)

Selecting a user uses the default dialog screens:

![createuser5](http://192.168.40.25:8081/wp-content/uploads/2009/06/createuser5.png)

In the last step the Wizard creates the new User Account including Home- and (Terminal Server) Profile Directories (including NTFS permissions), Mailbox, Group Membership and all needed properties. Even the Helpdesk Group Account is granted permissions on the Mailbox and Directories.

The next item in the Treeview is printers, so what can we do with them?

First of all we can list all network printers by simply expanding the Treeview, instantly we can see how many documents are in queue and errors such as paper out

![printers](http://192.168.40.25:8081/wp-content/uploads/2009/06/printers.png)

By Double Clicking we can open a specific printer (queue):

![printerprops](http://192.168.40.25:8081/wp-content/uploads/2009/06/printerprops.png)

We can even add new Network Pinters with a wizard similar to the create user wizard!

If we go down in the Treeview we see the Terminal Servers node which by default lists all Terminal Servers and/or Citrix Servers on the network. It does this very fast compared to TSAdmin:

![servertree](http://192.168.40.25:8081/wp-content/uploads/2009/06/servertree.png)

By DoubleClicking a server we can view details such as Processes:

![process](http://192.168.40.25:8081/wp-content/uploads/2009/06/process.png)

And Sessions:

![session](http://192.168.40.25:8081/wp-content/uploads/2009/06/session.png)

All operations are supported like Kill Process, Shadow Session (Dis)Connect, Logoff and so on. Each server connection runs in a seperate thread to ensure fast user response within the console.

If you are interested in a custom version of this console, please contact me (r dot weijnen at gmail dot com).