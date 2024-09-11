---
id: 432
title: 'Slow Installation because of Certificate Checks'
date: '2009-11-06T21:29:15+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=432'
permalink: /2009/11/06/slow-installation-because-of-certificate-checks/
views:
    - '5488'
short-url:
    - 'http://bit.ly/h77z4N'
categories:
    - Altiris
    - Citrix
    - 'Windows 2008'
---

A few days ago I noticed that an unattended installation of Citrix XenApp 5 was installing very slowly. When I looked at the various jobs and their installation time in the (Altiris) Deployment Server I saw that it was the Citrix Access Management Console that took almost 45 minutes to install:

[![AMInstallation](http://192.168.40.25:8081/wp-content/uploads/2009/11/aminstallation-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/11/aminstallation.png)

It was clear that this wasn’t normal since the install job is taking installing OS components like IIS and all subcomponents, activating Application Server and reboot in around 9 minutes. The installation of Citrix XenApp itsself takes only 14 minutes.

I searched with Google and one of the first links was this knowledge base article from Citrix: [Slow Access Management Console Installation on XenApp 5.0](http://support.citrix.com/article/CTX120429). The article clearly describes that the delay is occurred by failing checks for Publisher’s and Server Certificate Revocation (because there’s no Internet Connection) and suggests to turn these checks off. Indeed my servers do not have a direct internet connection so the cause and solution were clear.

And actually I had seen similar issues before in other (non Citrix) installations, some examples are Exchange 2007 ([here](http://support.microsoft.com/kb/944752) and [here](http://msexchangeteam.com/archive/2008/07/08/449159.aspx)) and SQL Server 2008 (the SQL Installer actually checks if there’s an internet connection in the prerequisites check).

The suggested, manual way, of turning of these checks is to clear the following checkboxes in Internet Explorer’s advanced settings Dialog:

[![IECertificateRevocation](http://192.168.40.25:8081/wp-content/uploads/2009/11/iecertificaterevocation-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/11/iecertificaterevocation.png)

But since I had to do this on many servers I decided it would be better to do it with a little VBS script.

I couldn’t find any documentation on where these settings are stored in the registry so I used [Process Monitor](http://technet.microsoft.com/en-us/sysinternals/bb896645.aspx) and [RegShot](http://sourceforge.net/projects/regshot/) to determine it. I found that the first setting (Check for publisher’s certificate revocation) is stored in “HKCU\\Software\\Microsoft\\Windows\\CurrentVersion\\WinTrust\\Trust Providers\\Software Publishing” in a REG\_DWORD value State. When turned it off (it’s on by default) it has a value of 146944, when turned on it has a value of 146432. I concluded that the State value is a combined value for several settings and that this particular setting has the value 512. I think it wouldn’t be wise to hard code the values 146944 and 146432 since I have no idea what other settings might be influenced, adding or subtracting this value is also unsafe since we don’t know if the value is on or off with different values. So the safest approach is to test with logical operators (and/or).

This is my code to test if the value is on or off:  
“&gt;  
And this is the code to set or unset the value:  
“&gt;  
The registry Key for the Check for server certificate revocation is simpler, it’s stored in “HKCU\\Software\\Microsoft\\Windows\\CurrentVersion\\Internet Settings\\CertificateRevocation” as a REG\_DWORD value with the values 0 for off and 1 for on. So the Get and Set functions are really simple:  
“&gt;  
And here an example of using the functions:  
“&gt;  
If you would like to use the script the link is below, please leave a comment if you think it’s usefull.

[ ChkPubRevocation.zip (1796 downloads ) ](http://192.168.40.25:8081/download/chkpubrevocation-zip/?tmstv=1726048918 "Version 1.0")