---
id: 2778
title: 'Application causes Screen Flickering in Citrix XenApp Session'
date: '2012-11-30T13:28:54+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2778'
permalink: /2012/11/30/application-causes-screen-flickering-in-citrix-xenapp-session/
views:
    - '15841'
short-url:
    - 'http://bit.ly/X7O4g2'
categories:
    - Citrix
    - 'Windows 2003'
tags:
    - Citrix
    - Hotfix
    - Microsoft
    - Server2003
    - WPF
    - XenApp
---

Yesterday I was asked to troubleshoot an interesting issue with an application running on Citrix XenApp.

**<span style="text-decoration: underline;">Environment  
</span>**This customer is running Citrix XenApp 5 on Windows Server 2003 (x86). On the Client Side the Online Plugin version 12.3 is used.

<span style="text-decoration: underline;">**The Problem**</span>  
When this particular application was active the screen was flickering and black blocks appeared at seemingly random places. Further more it was not possible to resize the window:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/11/image_thumb.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/11/image.png)

My experience is that display issues are often related to either HDX Flash Redirection (offloading flash to the client) or the Multi Monitor hook.

**<span style="text-decoration: underline;">HDX Flash Redirection</span>**  
XenApp allows us to selectively enable or disable HDX Flash Redirection per URL by using either a whitelist or a blacklist. This is usually done with a GPO.

I first tested if the issue was caused by HDX Flash Redirection by simply starting the session from within an RDP session but the problem still occurred.

<span style="text-decoration: underline;">**Multi Monitor Hook**</span>  
When you are using multiple monitors with Citrix XenApp both screens are actually one big screen with the XenApp session. XenApp then uses a hook mechanism (DLL Injection) to simulate multiple monitors.

Without these hooks you will for example notice that an application doesn’t start on one of the monitors but on all or is centered on them.

To quickly test if this issue was caused by the Multi Monitor hook I configured the ExcludeImageNames registry key as described in [CTX107825](http://support.citrix.com/article/CTX107825) which disables all hooks.

Since the problem still occurred I concluded the issue was not caused by the hooks.

**<span style="text-decoration: underline;">Windows Presentation Foundation</span>**  
Because I needed to find the process names for the afore mentioned ExcludedImageNames key I noticed that there was a process called Presentationhost.exe:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/11/image_thumb1.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/11/image1.png)

A google search for Presentationhost.exe tells us that it’s the host process for Windows Presentation Foundation (WPF) applications that are hosted in a browser.

**<span style="text-decoration: underline;">Hotfix  
</span>**So my next search was “Windows Presentation Foundation Screen Flicker” and the first hit was Microsoft [kb955692](http://support.microsoft.com/kb/955692): “Your screen flickers when you start WPF applications in a Windows Server 2003 terminal server session”.

- The hotfix from the article addresses an issue with WPF applications  
    You use a non-administrator user account to connect to a Windows Server 2003-base terminal server by using a third-party terminal client, such as a Citrix ICA client.
- You run Windows Presentation Foundation (WPF) applications in the terminal server session.

The hotfix fixes the issue however:

Considering that WPF and the related ClickOnce Deployment were meant to enable users without administrative privileges to install and run web applications this is really a stoopid bug, in summary: **WPF applications fail without write permissions in the %windir%\\system32 folder!**

**<span style="text-decoration: underline;">ClickOnce  
</span>**[ClickOnce](http://msdn.microsoft.com/en-us/library/t71a733d.aspx) is an even bigger disaster in SBC because for every user the whole application (6 MegaBytes in this case) is downloaded into **%userprofile%\\Local Settings\\Apps**:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/11/image_thumb2.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/11/image2.png)

This not only means that every user gets a copy of the application (which is bad enough in it’s self) but because the path is into Local Settings it means that **every user gets a copy on each server he/she logs into!**