---
id: 1833
title: 'Crack WEP Encryption'
date: '2011-06-14T11:55:39+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1833'
permalink: /2011/06/14/crack-wep-encryption/
short-url:
    - 'http://bit.ly/iwhVGq'
views:
    - '7592'
categories:
    - Uncategorized
tags:
    - back|track
    - Crack
    - Linux
    - WEP
---

I think everybody knows that using [WEP](http://en.wikipedia.org/wiki/Wired_Equivalent_Privacy) to encrypt your WiFi network is not very safe. To demonstrate this I will show you how easy it is to crack the WEP encryption in this post.

Note that I am using my own Access Point here so I am not actually cracking someone else’s WEP Key.

Requirements:

- <span style="color: #35383d;">[back|track Linux distribution](http://www.backtrack-linux.org/downloads/)</span>
- <span style="color: #35383d;">USB WiFi card (most internal WiFi cards will not work)</span>

5. 
6. In this post I am using the 32 bit back|track 5 VMWare image which you can use with VMWare Workstation or VMWare player.[![back|track downloads](http://192.168.40.25:8081/wp-content/uploads/2011/06/image_thumb.png "back|track downloads")](http://192.168.40.25:8081/wp-content/uploads/2011/06/image.png)After starting the back track Virtual Machine you can login with username **root** and password **toor**
    
    [![back track 5 logon screen](http://192.168.40.25:8081/wp-content/uploads/2011/06/image_thumb1.png "back track 5 logon screen")](http://192.168.40.25:8081/wp-content/uploads/2011/06/image1.png)
    
    Then type **startx** to start the X Window System (the Graphical Interface):
    
    [![back track logon screen](http://192.168.40.25:8081/wp-content/uploads/2011/06/image_thumb2.png "back track logon screen")](http://192.168.40.25:8081/wp-content/uploads/2011/06/image2.png)
    
    Before you go on, you need to Connect your WiFi card to the Virtual Machine using the Removable Devices menu:
    
    [![VMWare Removable Devices Menu](http://192.168.40.25:8081/wp-content/uploads/2011/06/image_thumb3.png "VMWare Removable Devices Menu")](http://192.168.40.25:8081/wp-content/uploads/2011/06/image3.png)
    
    Now start a Terminal using the Icon in the top bar and verify that your WiFi card is visible to back track using the command  
    “&gt;  
    Note the interface name, I will assume it’s *wlan0* from here.
    
    Then enable this interface for monitoring with the following command:  
    “&gt;  
    [![image](http://192.168.40.25:8081/wp-content/uploads/2011/06/image_thumb10.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/06/image10.png)
    
    Monitoring is now enabled on a special interface, I will assume it’s *mon0* from here*.*
    
    First we will see which networks are available:  
    “&gt;  
    [![airodump-ng mon0](http://192.168.40.25:8081/wp-content/uploads/2011/06/image_thumb5.png "airodump-ng mon0")](http://192.168.40.25:8081/wp-content/uploads/2011/06/image5.png)
    
    Wait a little while to get a list of the available networks and their encryption types. This post is about WEP encryption so look for a network that has WEP in the ENC column:
    
    [![image](http://192.168.40.25:8081/wp-content/uploads/2011/06/image_thumb11.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/06/image11.png)
    
    We need the BSSID and the Channel in the next command:  
    “&gt;  
    [![airodump capture traffic](http://192.168.40.25:8081/wp-content/uploads/2011/06/image_thumb7.png "airodump capture traffic")](http://192.168.40.25:8081/wp-content/uploads/2011/06/image7.png)
    
    Now we are capturing packets and we need about 20.000 data packets so just let it run for a while (note that there’s needs to be traffic in order to get data packets):
    
    [![airodump data packets](http://192.168.40.25:8081/wp-content/uploads/2011/06/image_thumb8.png "airodump data packets")](http://192.168.40.25:8081/wp-content/uploads/2011/06/image8.png)
    
    When there are enough packets captured we can stop the capture with ctrl-c. Use the dir or ls command to view the generated files, we need the wepkey-01.cap file in this case.
    
    The actual decyphering of the key is done with the command:  
    “&gt;  
    [![aircrack captured wep key](http://192.168.40.25:8081/wp-content/uploads/2011/06/image_thumb9.png "aircrack captured wep key")](http://192.168.40.25:8081/wp-content/uploads/2011/06/image9.png)
    
    aircrack returns almost immediately and found the key “12345678ab” which is correct:
    
    [![iPhone MyWi WEP Key](http://192.168.40.25:8081/wp-content/uploads/2011/06/005_thumb.png "iPhone MyWi WEP Key")](http://192.168.40.25:8081/wp-content/uploads/2011/06/005.png)
    
    Conclusion: You shouldn’t use WEP since it can be hacked within a few minutes.