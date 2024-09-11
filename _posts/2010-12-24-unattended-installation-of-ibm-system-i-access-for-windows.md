---
id: 951
title: 'Unattended Installation of IBM System i Access for Windows'
date: '2010-12-24T15:52:04+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=951'
permalink: /2010/12/24/unattended-installation-of-ibm-system-i-access-for-windows/
views:
    - '17168'
short-url:
    - 'http://bit.ly/fyBZzV'
categories:
    - Altiris
    - script
---

Today I needed to script the installation of IBM System i Access for Windows (formerly called IBM Client Access).

With older versions of this client (up to 5.4) you could use the -r (record) installer switch to record the install in a setup.iss file but version 6.1 uses an MSI based installer.

IBM offers the client in a 2 DVD download but you probably only need the first dvd (dvd 1 has both the x86 and x64 installers, dvd 2 has the ia64 installer) which is a whopping 3,5 GB download.

Inside the download (a zip) is an iso file of which you will only need the files in the root and the Image32 or Image64a folder.

Inside the image folder are subfolders names MRI29xx where xx is a language identifier:

The following Identifiers are used:

> MRI2912 – Croatian  
> MRI2922 – Portuguese  
> MRI2923 – Dutch Netherlands  
> MRI2924 – English  
> MRI2925 – Finnish  
> MRI2926 – Danish  
> MRI2928 – French  
> MRI2929 – German  
> MRI2930 – Japanese  
> MRI2931 – Spanish  
> MRI2932 – Italian  
> MRI2933 – Norwegian  
> MRI2937 – Swedish  
> MRI2938 – English Uppercase DBCS  
> MRI2939 – German Multinational  
> MRI2940 – French Multinational  
> MRI2942 – Italian Multinational  
> MRI2954 – Arabic  
> MRI2956 – Turkish  
> MRI2957 – Greek  
> MRI2961 – Hebrew  
> MRI2962 – Japanese (5026)  
> MRI2963 – Belgian Dutch Multinational  
> MRI2966 – Belgian French Multinational  
> MRI2975 – Czech  
> MRI2976 – Hungarian  
> MRI2978 – Polish  
> MRI2979 – Russian  
> MRI2980 – Brazilian Portuguese  
> MRI2981 – Canadian French Multinational  
> MRI2984 – English DBCS  
> MRI2986 – Korean DBCS (KS)  
> MRI2987 – Traditional Chinese DBCS  
> MRI2989 – Simplified Chinese DBCS (GB)  
> MRI2992 – Romanian  
> MRI2994 – Slovakian  
> MRI2996 – Portuguese Multinational

So if you need to transfer the installer over a slow connection you only need to transfer the language(s) you require.

To set the language you can use the CWBPRIMARYLANG property to set the language eg CWBPRIMARYLANG=Mri2923 and the /v switch can be used to pass MSI Public properties to the installer.

Note that the installer requires a reboot so you may want to add REBOOT=ReallySupress to prevent this.

I use [Master Return Codes](http://192.168.40.25:8081/2010/12/24/master-return-codes-in-altiris/ "Master Return Codes in Altiris") for this in Altiris.

This the Altiris Job I use:

“&gt;  
IBM References:

- <div>[Public Properties](http://publib.boulder.ibm.com/infocenter/iseries/v6r1m0/topic/rzaij/rzaijpblcprop.htm "Public Properties")</div>
- <div>[Defining the level of user interface throughout the installation](http://publib.boulder.ibm.com/infocenter/iseries/v6r1m0/topic/rzaij/rzaigdfnlvlusriface.htm "Defining the level of user interface throughout the installation")</div>
- <div>[Using command line parameters to change the installation behaviour](http://publib.boulder.ibm.com/infocenter/iseries/v6r1m0/topic/rzaij/rzaijspecfeatetc.htm "Using command line parameters to change the installation behaviour")</div>

Other references:

- <div>[SQL Query to get the Full Path of an Altiris Job](http://192.168.40.25:8081/2010/12/12/sql-query-to-get-the-full-path-of-an-altiris-job-3/ "SQL Query to get the Full Path of an Altiris Job")</div>
- <div>[Using Master Return Codes in Altiris](http://192.168.40.25:8081/2010/12/24/master-return-codes-in-altiris/ "Using Master Return Codes in Altiris")</div>