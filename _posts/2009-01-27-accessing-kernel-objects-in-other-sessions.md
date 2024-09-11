---
id: 237
title: 'Accessing kernel objects in other sessions part 1'
date: '2009-01-27T19:43:52+01:00'
author: daNIL
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2009/01/27/accessing-kernel-objects-in-other-sessions/'
permalink: /2009/01/27/accessing-kernel-objects-in-other-sessions/
views:
    - '9486'
short-url:
    - 'http://bit.ly/hIjkm3'
categories:
    - Delphi
    - General
    - Programming
    - 'Terminal Server'
---

As you know, many *kernel32.dll* functions, which are working with named objects, like *OpenEvent*, can be used to work with global and local objects. So what are global and local objects? Global objects are created in session 0 and are actually located in the **\\BaseNamedObjects** directory, while local objects are created in the caller’s session (for example in the **\\Sessions\\5\\BaseNamedObjects** directory (for session 0, global and local has no meaning since they point to the same object)). MSDN says that you can access only the objects in your own session(via the **Local\\** prefix) and in session 0 (via the **Global\\** prefix). But what if you need to access an object in another session? There is nothing in MSDN about it, only [this](http://msdn.microsoft.com/en-us/library/aa382954(VS.85).aspx) page says “*<font color="blue">The “Local”, “Global”, and “Session” prefixes are reserved for system use and should not be used as names for kernel objects.</font>*” Let’s look at it via [WinObj](http://technet.microsoft.com/en-us/sysinternals/bb896657.aspx) tool:

[![session3](http://192.168.40.25:8081/wp-content/uploads/2009/01/session3-small.gif)](http://192.168.40.25:8081/wp-content/uploads/2009/01/session3.gif)

As you see, we’re now in session 3. **\\Sessions\\3\\BaseNamedObjects** contains 3 links: **Global**, which points to **\\BaseNamedObjects**, **Local**, which just points to itself (**\\Sessions\\3\\BaseNamedObjects**), and **Session**, which points to **\\Sessions\\BNOLINKS**. Let’s look at the content of the **BNOLINKS** directory:

[![BNOLinks](http://192.168.40.25:8081/wp-content/uploads/2009/01/bnolinks-small.gif)](http://192.168.40.25:8081/wp-content/uploads/2009/01/bnolinks.gif)

Aha, so it seems that this directory just contains the links to the other sessions’ **BaseNamedObjects** directories (I think, BNOLINKS stands for Base Named Objects Links)! So if you need to access an object in, for example, session 2, you can open it using the **Session** link: “**Session\\2\\ObjectName**“. At first, **Session** will be resolved to **\\Sessions\\BNOLINKS\\2\\ObjectName**, then **\\Sessions\\BNOLINKS\\2** will be resolved to **\\Sessions\\2\\BaseNamedObjects\\ObjectName**, which means, that we’re now accessing object **ObjectName** of session 2!

Let’s write a simple program, which will create and open objects in other sessions:

At first, we need to get the session list:  
“&gt;  
Then we can create objects in other sessions:  
“&gt;  
And then we can try to open the objects in other sessions:  
“&gt;  
[ Session Objects (1835 downloads ) ](http://192.168.40.25:8081/download/session-objects/?tmstv=1726048918 "Version 1.0")

P.S. You can use this technique in any session for any object, which is created in the **BaseNamedObjects** directory: *Event, Mutex, Semaphore, WaitableTimer, Job, FileMapping*. Of course, security restrictions apply: you cannot create any object in another session if you do not have access to it. The *LocalSystem* account can create/access objects in any session, even idle sessions (which have not been initialized yet)

P.P.S. If terminal services are not running, the **Session** and **\\Sessions** links are not created, so you cannot use them (but you can still use the **Global** and **Local** links, which are always created). Actually, there is no sense of using the **Session\\0\\** prefix, since you can just use the **Global\\** prefix.

These are not the only places where you can create the kernel objects (Check [part 2](/blog/2009/02/03/accessing-kernel-objects-in-other-sessions-part-2/)).