---
id: 2621
title: 'DefaultPassword Dumper'
date: '2012-05-21T22:39:05+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2012/05/21/defaultpassword-dumper/'
permalink: /2012/05/21/defaultpassword-dumper/
views:
    - '2636'
short-url:
    - 'http://bit.ly/Kt68aM'
categories:
    - 'Windows Internals'
---

Just a small post today: a small commandline utility that reads the “DefaultPassword” LSA secret.

This secret is stored in the registry under the SECURITY Hive:

[![HKEY_LOCAL_MACHINE\SECURITY\Policy\Secrets\DefaultPassword](http://192.168.40.25:8081/wp-content/uploads/2012/05/SNAGHTML34f1d213_thumb.png "RegEdit")](http://192.168.40.25:8081/wp-content/uploads/2012/05/SNAGHTML34f1d213.png)

It’s data is encrypted but can be read using the LsaRetrievePrivateData API.

<del>It’s meaning is not documented but the value is often the Windows account password but not always 😀</del>

EDIT: as Andrew Morgan commented the DefaultPassword key is of course the encrypted version of the Autologon password as set by for example the SysInternals Autologon tool. I really should have known that…

[![DefaultPassword from LSA Secrets](http://192.168.40.25:8081/wp-content/uploads/2012/05/image_thumb5.png "cmd.exe")](http://192.168.40.25:8081/wp-content/uploads/2012/05/image5.png)

[ DefaultPassword.zip (1593 downloads ) ](http://192.168.40.25:8081/download/defaultpassword-zip/?tmstv=1726048919 "Version 1.0")