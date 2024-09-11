---
id: 622
title: 'Compatibility Issue with RAD Studio XE'
date: '2010-09-02T19:50:42+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2010/09/02/compatibility-issue-with-rad-studio-xe/'
permalink: /2010/09/02/compatibility-issue-with-rad-studio-xe/
views:
    - '4803'
short-url:
    - 'http://bit.ly/eib9xe'
categories:
    - Delphi
---

After launching the newly installed RAD Studio XE for the first time it tried to install something. This failed because I didn’t run it elevated which makes Windows 7 fire the Program Compatibility Assistant:

![RadStudioXECompat](http://192.168.40.25:8081/wp-content/uploads/2010/09/radstudioxecompat.png)

It would be better for Embarcadero to detect if we run elevated and only run the installer when we are (or request elevation).

Maybe it’s time for Embarcadero to use Jwscl which make such things very easy?