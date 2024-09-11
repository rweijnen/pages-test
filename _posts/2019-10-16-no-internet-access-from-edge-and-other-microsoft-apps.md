---
id: 4335
title: 'No internet access from Edge and other Microsoft apps'
date: '2019-10-16T15:30:57+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://192.168.40.25:8081/?p=4335'
permalink: /2019/10/16/no-internet-access-from-edge-and-other-microsoft-apps/
views:
    - '121'
categories:
    - Uncategorized
---

I had a strange issue today: I couldn’t open any webpage in the Edge browser on my Windows 10 machine:

<figure class="wp-block-image">![Edge browser with error message "Can't reach this page"](http://192.168.40.25:8081/wp-content/uploads/2019/10/image.png)<figcaption>Edge browser: “Can’t reach this page”</figcaption></figure><div class="wp-block-image"><figure class="alignleft">![Network icon showing Internet access](http://192.168.40.25:8081/wp-content/uploads/2019/10/image-1.png)</figure></div>The network icon was showing that there was Internet access and a quick check on the command prompt showed that the connection (including name resolution appeared to work fine):

<figure class="wp-block-image">![Command prompt showing that ping to www.google.com works fine.

ping www.google.com

Pinging www.google.com [172.217.168.196] with 32 bytes of data:
Reply from 172.217.168.196: bytes=32 time=15ms TTL=55
Reply from 172.217.168.196: bytes=32 time=14ms TTL=55

Ping statistics for 172.217.168.196:
    Packets: Sent = 2, Received = 2, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 14ms, Maximum = 15ms, Average = 14ms](http://192.168.40.25:8081/wp-content/uploads/2019/10/image-2.png)<figcaption>ping to www.google.com</figcaption></figure>Other browsers such as Firefox (my default browser) and Chrome also worked fine but some other Microsoft applications also didn’t work such as the Microsoft Store:

<figure class="wp-block-image">![Microsoft Store: Try that again
Page could not be loaded. Please try again later.
Code: 0x80131500](http://192.168.40.25:8081/wp-content/uploads/2019/10/image-3.png)<figcaption>Microsoft Store app “Try that again”</figcaption></figure>And Outlook which seemed to be stuck at “Loading Profile”:

<figure class="wp-block-image">![Outlook splash screen hung at "Loading Profile"](http://192.168.40.25:8081/wp-content/uploads/2019/10/image-4.png)<figcaption>Outlook hangs at “Loading Profile”</figcaption></figure>Strangely enough good old Internet Explorer also worked fine (it opens on the Welcome page as I never use it):

<figure class="wp-block-image">![Internet Explorer showing the "Welcome to the web" page:
You should try Microsoft Edge


The newest browser from Microsoft for Windows 10




The same people who brought you Internet Explorer proudly bring you Microsoft Edge. It builds on the Internet Explorer browser you trust and lets you take along all your favorites. Plus you can:



•Write on web pages 
•Read without distractions 
•Get instant search results 

Go beyond browsing with Microsoft Edge 

TRY MICROSOFT EDGE NOW ](http://192.168.40.25:8081/wp-content/uploads/2019/10/image-5.png)<figcaption>Internet Explorer works fine</figcaption></figure>Ironically Internet Explorer recommends to use Edge 😂

I then noticed that my internet connection was marked as “Public network” even though I am sure that it was previously set to “Private”:

<figure class="wp-block-image">![Network Status showing the network is marked as a Public network](http://192.168.40.25:8081/wp-content/uploads/2019/10/image-6.png)<figcaption>Network Status</figcaption></figure>When I switched the network to Private, Edge immediately loaded the page and the Outlook splash screen disappeared and it’s main window popped up.

Unfortunately I do not have an explanation (yet) for this strange error, I can only say it happened after updating VMware Workstation which may or may note have anything to do with it.

EDIT: 18-10-2019: Switched back to Public profile and everything stays working including Edge and Outlook.

I’ll update this post if/when I find out more…