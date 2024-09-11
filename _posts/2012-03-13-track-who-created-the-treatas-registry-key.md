---
id: 2542
title: 'Track who created the TreatAs registry key'
date: '2012-03-13T13:41:56+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2012/03/13/track-who-created-the-treatas-registry-key/'
permalink: /2012/03/13/track-who-created-the-treatas-registry-key/
views:
    - '1785'
short-url:
    - 'http://bit.ly/AvIoGq'
categories:
    - Other
    - RES
tags:
    - 'Automation Manager'
    - SharePoint
---

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/03/image_thumb14.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/03/image14.png)Last week I wrote about an error message the users received when [opening documents from SharePoint](http://192.168.40.25:8081/2012/03/09/edit-document-requires-a-windows-sharepoint-services-compatible-application/).

The article showed how to fix the problem but it didn’t feel good that I didn’t know where this “TreatAs” value was coming from.

I figured that I could read the timestamp key from the registry to see at what/date time the value was created. This value can be read using the [RegQueryInfoKey](http://msdn.microsoft.com/en-us/library/ms724902%28VS.85%29.aspx) API but there are various tools that can read it.

I used the [RegScanner](http://www.nirsoft.net/utils/regscanner.html) tool from NirSoft:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/03/image_thumb15.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/03/image15.png)

Since the functionality is related to Office I suspected that the “TreatAs” value was created by the “Microsoft Office 2010 Deployment Kit for App-V” that was recently introduced into the enviroment.

So I tracked the deployment history in Automation Manager via the Job Results. I selected the Deployment Kit Module and clicked Details:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/03/image_thumb16.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/03/image16.png)

And Details again in the Next screen:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/03/image_thumb17.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/03/image17.png)

Then I went to the Log tab where I could see that the Value was indeed created during installation of the Deployment Kit for App-V:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/03/image_thumb18.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/03/image18.png)

The Value in the TreatAs Value seems related to the SharePoint Proxy registry key as mentioned in the document “[Prescriptive guidance for sequencing Office 2010 using Microsoft App-V 4.5 or 4.6](http://support.microsoft.com/kb/983462)“.

I was using the following parameters:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/03/image_thumb19.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/03/image19.png)

So if you are deploying the Office 2010 Deployment Kit for App-V watch out for this Value and test with SharePoint!