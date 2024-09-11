---
id: 3458
title: 'Error MSB4018 when building a Management Pack with Visual Studio Authoring Extensions'
date: '2014-07-08T14:38:56+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3458'
permalink: /2014/07/08/error-msb4018-when-building-a-visual-studio-authoring-extensions/
views:
    - '751'
short-url:
    - 'http://bit.ly/1pWPo4C'
    - 'http://bit.ly/1pWPo4C'
categories:
    - 'Visual Studio'
tags:
    - 'Authoring Extensions'
    - Build
    - 'Operations Manager'
    - SCOM
    - 'System Center'
    - 'Visual Studio'
---

I am currently working on a Management Pack for System Center Operations Manager (aka SCOM). I am using the [System Center 2012 Visual Studio Authoring Extensions](http://www.microsoft.com/en-us/download/details.aspx?id=30169) and during build of my project I suddenly got the following error: “*MSB4018: The “MergeFragments” task failed unexpectedly*“:

[![Visual Studio | System Center 2012 Visual Studio Authoring Extensions | C:\Program Files (x86)\MSBuild\Microsoft\VSAC\Microsoft.SystemCenter.OperationsManager.targets(234,5) | error MSB4018: The "MergeFragments" task failed unexpectedly.](http://192.168.40.25:8081/wp-content/uploads/2014/07/errorMSB4018_thumb.png "error MSB4018: The "MergeFragments" task failed unexpectedly.")](http://192.168.40.25:8081/wp-content/uploads/2014/07/errorMSB4018.png)

I searched on this error message but wasn’t able to find anything helpful. In order to get more detailed output from MSBuild I changed the MSBuild project build output verbosity. To do this go to the Tools menu in Visual Studio and select Options. Navigate to the “Build and Run” node under “Projects and Solutions” and set both options to “Diagnostic”:

[![Visual Studio | Tools | Options | Projects and Solutions | Build and Run | MSBuild project build output verbosity | Diagnostic](http://192.168.40.25:8081/wp-content/uploads/2014/07/VisualStudioBuildOptions_thumb.png "Build Options")](http://192.168.40.25:8081/wp-content/uploads/2014/07/VisualStudioBuildOptions.png)

That gave a more detailed output in the Build window where I noticed that two References had the same Alias:

[![Build Output | Duplicate Reference](http://192.168.40.25:8081/wp-content/uploads/2014/07/BuildOutput_thumb.png "Build utput")](http://192.168.40.25:8081/wp-content/uploads/2014/07/BuildOutput.png)

I then changed the Alias for one of those References in the Solution Explorer:

[![Visual Studio | Solution Explorer | References | Alias](http://192.168.40.25:8081/wp-content/uploads/2014/07/VisualStudioSolutionExplorer_thumb.png "Visual Studio | Solution Explorer")](http://192.168.40.25:8081/wp-content/uploads/2014/07/VisualStudioSolutionExplorer.png)

That solved the error!