---
id: 1311
title: 'Saving Password in Office Communicator'
date: '2011-01-27T15:50:27+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/01/27/saving-password-in-office-communicator/'
permalink: /2011/01/27/saving-password-in-office-communicator/
views:
    - '2897'
short-url:
    - 'http://bit.ly/fMab6I'
categories:
    - General
---

If you want to be able save the password in Office Communicator you must create the key *HKLM\\Software\\Microsoft\\Communicator* or on x64 OS *HKLM\\Software\\Wow6432Node\\Policies\\Microsoft\\Communicator* and set the DWORD value SavePassword to 1.

Now you will have the Save my password checkbox (which will save the encrpted password to HKCU\\Software\\Microsoft\\Communicator\\AccountPassword

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/01/image_thumb20.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/01/image20.png)