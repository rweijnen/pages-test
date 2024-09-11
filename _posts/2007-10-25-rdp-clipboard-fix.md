---
id: 37
title: 'RDP Clipboard Fix'
date: '2007-10-25T17:45:57+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2007/10/25/rdp-clipboard-fix/'
permalink: /2007/10/25/rdp-clipboard-fix/
views:
    - '94824'
short-url:
    - 'http://bit.ly/hu7bcp'
categories:
    - Citrix
    - Programming
    - 'Terminal Server'
---

Did you ever loose Clipboard functionality (copy/paste) while working with several Terminal Server sessions? I think everyone that works a lot with Terminal Server has experienced this from time to time.

It’s caused by badly behaving applications. Dimitry Vostokov wrote a tool to fix this issue for Citrix (RepairCBDChain.exe), he explains the issue very well on his blog:

*Windows has a mechanism to notify applications about clipboard changes. An application interested in such notifications has to register itself in the so called clipboard chain. Windows inserts it on top of that chain and that application is responsible to propagate changes down the chain:*

[*![rc1.JPG](http://citrite.org/blogs/dmitryv/files/2006/12/rc1.thumbnail.JPG)*](http://citrite.org/blogs/dmitryv/files/2006/12/rc1.JPG)

*If 3rd-party application forgets to forward notifications down then we have a broken clipboard chain and clipboard changes are not sent via ICA protocol:*

Read more at Dimitry’s Blog: <http://citrite.org/blogs/dmitryv/2006/12/09/clipboard-issues-explained/>

So how can we fix this for Terminal Server then?  
   
MSTSC creates a hidden window with Window Class RdpClipRdrWindowClass, you can observe this with a Window Spy tool such as [X-Spy](http://www.x-spy.net/index.php?lang=en&site=index)

![Clipboard Redirector Window](http://192.168.40.25:8081/wp-content/uploads/2007/10/rdpcliprdrwindow1.png)

If we look at the Window Style we can see the Window is hidden (doesn’t contain WS\_VISIBLE):

[![Window Style](http://192.168.40.25:8081/wp-content/uploads/2007/10/windowstyle1.png)](http://192.168.40.25:8081/2007/10/25/rdp-clipboard-fix/window-style/ "Window Style")

So we just need to find all windows with this Windows Class and subscribe them to Clipboard Notifications.

The code is simple:  
“&gt;  
Using EnumWindows instead of FindWindow (as RepairCBDChain seems to do) has the advantage that it finds multiple windows (when you have multiple sessions) and not just the first.

You can download the compiled version below. If you like it why not leave a *comment*?

![Screenshot RDPFixClip](http://192.168.40.25:8081/wp-content/uploads/2007/10/rdpfixclip1.png)

[ RDP Clipboard Fix (14445 downloads ) ](http://192.168.40.25:8081/download/rdp-clipboard-fix/?tmstv=1726048918 "Version 1.0")

See here for more Terminal Server related articles: <http://192.168.40.25:8081/topics/terminalserver/>

There’s an explanation of this issue at the Microsoft Terminal Services team blog too:  
<http://blogs.msdn.com/ts/archive/2006/11/16/why-does-my-shared-clipboard-not-work-part-1.aspx>  
<http://blogs.msdn.com/ts/archive/2006/11/20/why-does-my-shared-clipboard-not-work-part-2.aspx>