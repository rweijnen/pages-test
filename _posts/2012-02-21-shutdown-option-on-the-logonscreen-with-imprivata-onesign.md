---
id: 2456
title: 'Shutdown option on the logonscreen with Imprivata Onesign'
date: '2012-02-21T16:12:12+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2012/02/21/shutdown-option-on-the-logonscreen-with-imprivata-onesign/'
permalink: /2012/02/21/shutdown-option-on-the-logonscreen-with-imprivata-onesign/
views:
    - '2301'
short-url:
    - 'http://bit.ly/ysyBN9'
categories:
    - General
tags:
    - Imprivata
---

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/02/image_thumb17.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/02/image17.png)I logged remotely to a server with RDP and I noticed that I had options to restart or shutdown that server. This means we can shutdown or restart a server without physical access and without authentication:

[![Windows Server 2003 Logon Screen | Imprivata | Shutdown | REstart](http://192.168.40.25:8081/wp-content/uploads/2012/02/image_thumb18.png "Log On to Windows")](http://192.168.40.25:8081/wp-content/uploads/2012/02/image18.png)

We can remove the Shut down and Restart hyperlink by setting the following REG\_DWORD value **UseShutDownControls** to 0 in the *HKLM\\SOFTWARE\\SSOProvider\\SuperGina* registry key.

So this is a clear case of misconfiguration, probably due to the fact that the installation script was copied from a workstation installation where you might want to allow this setting.

But even on a workstation you might not want to have those options when connecting to it remotely. So do consider carefully if you want to enable this setting.