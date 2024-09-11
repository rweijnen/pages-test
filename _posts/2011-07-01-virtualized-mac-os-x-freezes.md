---
id: 1965
title: 'Virtualized Mac OS X Freezes'
date: '2011-07-01T15:12:39+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/07/01/virtualized-mac-os-x-freezes/'
permalink: /2011/07/01/virtualized-mac-os-x-freezes/
views:
    - '2336'
short-url:
    - 'http://bit.ly/iN0YNy'
categories:
    - Apple
    - VMWare
tags:
    - Apple
    - MacOSX
    - VMWare
---

[![Mac OS X Snow Leopard](http://192.168.40.25:8081/wp-content/uploads/2011/07/image_thumb3.png "Mac OS X")](http://192.168.40.25:8081/wp-content/uploads/2011/07/image3.png)I am running a virtualized Mac OS X machine in my VMWare Workstation but I noticed that after a period of inactivity the virtual machine would sometimes freeze.

Because this only happens after inactivity I assumed it had something to do with Power Saving so I changed the Energy Saver settings and that fixed it!

Go to System Preferences:

[![System Preferences](http://192.168.40.25:8081/wp-content/uploads/2011/07/image_thumb.png "System Preferences")](http://192.168.40.25:8081/wp-content/uploads/2011/07/image.png)

Click Energy Saver:

[![Energy Settings](http://192.168.40.25:8081/wp-content/uploads/2011/07/image_thumb1.png "System Preferences")](http://192.168.40.25:8081/wp-content/uploads/2011/07/image1.png)

Set Computer sleep to Never:

[![System Preferences | Enery Saver | Computer Sleep | Never](http://192.168.40.25:8081/wp-content/uploads/2011/07/image_thumb2.png "Energy Saver")](http://192.168.40.25:8081/wp-content/uploads/2011/07/image2.png)