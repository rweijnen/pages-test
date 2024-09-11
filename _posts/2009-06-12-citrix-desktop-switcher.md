---
id: 352
title: 'Citrix Desktop Switcher'
date: '2009-06-12T12:40:09+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2009/06/12/citrix-desktop-switcher/'
permalink: /2009/06/12/citrix-desktop-switcher/
views:
    - '11119'
short-url:
    - 'http://bit.ly/hWT9s2'
categories:
    - Citrix
---

A while ago I wrote a small tool to assist in switching between a Full Screen Citrix Desktop and the local desktop. By default the Citrix client can switch from full screen to windowed mode (with the SHIFT F2 hotkey) but it doesn’t minimize the window automatically. So this always requires manually minimizing, do your local work, give focus to the Citrix client again and press the hotkey again to return to full screen.

My idea was really simply: we write a little exe that runs locally and registers the SHIFT F2 hotkey. When the Hotkey is pressed we determine if we are in full screen or in windowed mode and reverse that. When going from Full Screen to Windowed we minimize the Citrix Client and notify the user (by balloon tip) that he is on the local desktop. I called it the Citrix Desktop Switcher (sorry I couldn’t come up with a more original name)

So let’s see it in action!

When you start the Citrix Desktop Switcher you are notified that the tool is running (it doesn’t matter when you start the Switcher, you can start if even if the Citrix Session is already running).

![Balloon1](http://192.168.40.25:8081/wp-content/uploads/2009/06/balloon1.png)

Now we press the Hotkey SHIFT F2 and the Desktop Switcher kicks in and minimizes the Citrix Window for us and notifies the user:

![Balloon2](http://192.168.40.25:8081/wp-content/uploads/2009/06/balloon2.png)

If you want to return to the Citrix Session there’s no need to active the Citrix Client, just press the Hotkey again and there you are!

The Citrix Desktop Switcher can be exited with the Hotkey CTRL-F2 and it guards against accidental press by asking for your confirmation:

![Confirmation](http://192.168.40.25:8081/wp-content/uploads/2009/06/confirmation.png)

[ Citrix Desktop Switcher (2330 downloads ) ](http://192.168.40.25:8081/download/citrix-desktop-switcher/?tmstv=1726048918 "Version 1.0")