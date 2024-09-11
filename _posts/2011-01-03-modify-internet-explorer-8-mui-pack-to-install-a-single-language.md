---
id: 1055
title: 'Modify Internet Explorer 8 MUI Pack to install a single language'
date: '2011-01-03T12:10:22+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1055'
permalink: /2011/01/03/modify-internet-explorer-8-mui-pack-to-install-a-single-language/
views:
    - '4760'
short-url:
    - 'http://bit.ly/flYZuD'
categories:
    - General
    - Packaging
    - 'Unattended Installation'
    - 'Windows 2008'
    - 'Windows 2008 R2'
    - 'Windows 7'
    - 'Windows XP'
---

Today I wanted to install the Dutch Language pack for Internet Explorer 8, the Dutch language comes as part of the [Windows Internet Explorer 8 MUI Pack](http://www.microsoft.com/downloads/en/details.aspx?FamilyID=242bf57a-9dab-4ea9-ba46-33c0e32020a4&displaylang=en "Windows Internet Explorer 8 MUI Pack for Windows Server 2003 SP2") (in my case the version for Windows Server 2003 SP2).

If you install the MUI Pack you will always end up with all 35 (!) languages installed. This behaviour is the same as the language pack for Internet Explorer 7 that I wrote about earlier (see [Modifying Microsoft Updates and/or hotfixes](http://192.168.40.25:8081/2009/05/12/modifying-microsoft-updates-andor-hotfixes "Modifying Microsoft Updates and/or hotfixes"))

The solution is really the same as for the IE7 language pack: you modify the inf file (in my case update\_srv03.inf) but if you run update.exe it will refuse to use your modified inf file:

[![ie8muierror](http://192.168.40.25:8081/wp-content/uploads/2011/01/ie8muierror-small1.png)](http://192.168.40.25:8081/wp-content/uploads/2011/01/ie8muierror.png)

So we need to patch update.exe to accept your modified version!

The patcher from my previous article doesn’t work however on the update.exe from this MUI pack because the asm was changed too much for my patcher to recognise it.

So I created a new patch, based on update.exe 6.13.5.0 dated 7-1-2009.

The patcher will change this code:  
“&gt;  
to  
“&gt;  
So now you can install just the languages you need (see the previous article for a howto on editing the inf file).  
Enjoy and please leave a comment if you found this usefull!

[ Microsoft Update.exe Patcher (1001 downloads ) ](http://192.168.40.25:8081/download/microsoft-update-exe-patcher/?tmstv=1726048919)