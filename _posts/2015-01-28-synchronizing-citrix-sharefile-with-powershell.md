---
id: 3555
title: 'Synchronizing Citrix ShareFile with PowerShell'
date: '2015-01-28T11:39:17+01:00'
author: 'Remko Weijnen'
excerpt: 'Get enum names and values with PowerShell | Add a Citrix ShareFile Synchronization Job with PowerShell'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3555'
permalink: /2015/01/28/synchronizing-citrix-sharefile-with-powershell/
short-url:
    - 'http://bit.ly/1v2Yu30'
views:
    - '1407'
categories:
    - Citrix
    - PowerShell
    - ShareFile
tags:
    - Citrix
    - PowerShell
    - ShareFile
---

[![ShareFileLogo](http://192.168.40.25:8081/wp-content/uploads/2015/01/ShareFileLogo_thumb.gif "ShareFileLogo")](http://192.168.40.25:8081/wp-content/uploads/2015/01/ShareFileLogo.gif)The Citrix ShareFile Sync application is quite limited in functionality, one of those limitations is that you can only synchronize to a single (one) local folder.

As Helge Klein wrote in his excellent article "[Configuring Citrix ShareFile Sync from PowerShell](https://helgeklein.com/blog/2014/01/configuring-citrix-sharefile-sync-powershell/)" this is simply a GUI restriction and not a restriction in the actual ShareFile sync engine.

Helge describes that you can easily do this in PowerShell with the following example:

 “&gt;

While the command was accepted, nothing was synchronized.

I then created a Sync job with the Gui and noticed it had a different value for Authentication Type:

“&gt;

Notice that the AuthenticationType is not listed as an Integer but with it’s name. The job that I added however had AuthenticationType 4 while I expected oauth\_sharefile as per Helge’s article.

Let’s use PowerShell to tell us what the possible values are for AuthenticationType:

“&gt;

Here’s the output:

“&gt;

As you can see there is no value 4! Perhaps Helge used an older version of the Sharefile Windows Client or the Enterprise version. Shame on the developers though for documenting so badly and breaking stuff between versions and editions.

This is also shows two design principles of a REST API (which is what lies under the PowerShell commands) that I dislike:

- REST does not require the client to know anything about the structure of the API.
- REST expects the server to provide whatever information the client needs to interact with the service.

This means that there’s little type safety and that input validation is often lacking.

To complete this post let’s do the same trick for SyncDirection which type is

“&gt;

I changed the AuthType param to 0 and now my folder is synchronizing.

Happy synching!