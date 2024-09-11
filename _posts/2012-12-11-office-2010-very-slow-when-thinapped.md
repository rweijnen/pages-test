---
id: 2905
title: 'Office 2010 very slow when ThinApped'
date: '2012-12-11T09:00:11+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2905'
permalink: /2012/12/11/office-2010-very-slow-when-thinapped/
short-url:
    - 'http://bit.ly/YVSOY4'
views:
    - '4884'
categories:
    - ThinApp
tags:
    - 'Office 2010'
    - ThinApp
---

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb17.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image17.png)This blog is about an issue with VMware ThinApp and Office 2010 I discovered a while ago.

**<u>Environment</u>**   
Customer is using Office 2003 natively on a Citrix XenApp 5 environment. Some users had a business need for Office 2010, therefore a ThinApp with Office 2010 was created (this customer uses ThinApp for App-Virt).

<font color="#35383d">To make the picture complete: Thinapp version is 4.7.2-771812</font> <font color="#35383d">and Office version is 2010 SP1 (14.0.6024.1000)</font> .

**<u>Symptoms</u>**   
[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb18.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image18.png)Users complained that Office 2010 was very slow. Most noticeable was Outlook 2010 which was completely unusable.

Outlook startup time was minutes rather than seconds and while starting it seemed to delay on loading profile.

When Outlook was finally started, switching between folders and layouts felt really sluggish.

For example when switching from Calendar to Inbox took a few seconds, after which it would take almost 20 seconds for the e-mails to show. Switching between e-mail would take 2-3 seconds to display the contents in the reading pane.

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb19.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image19.png)Next I tested Word and noticed that Word seemed to work reasonably well when editing documents but was very slow when opening and saving documents.

Overall it seemed like Office was slow whenever there was network access.

I closed Outlook and restarted it with the [/RpcDiag](http://office.microsoft.com/en-us/outlook-help/command-line-switches-for-outlook-2010-HP010354956.aspx) commandline switch:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb20.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image20.png)

As you can see in the screenshot the average latency is extremely high for a fast, direct, LAN connection.

**<u>Testing   
</u>**I was surprised that in the testing process nobody noticed the slowness. So I went back to the packaging machine, a machine identical to production but virtual.

Strangely enough both Office and Outlook were not slow at all, even though the Virtual Machine was running with far less specs than the production servers.

I started Outlook with the /RpcDiag switch and noticed that the average RPC latency was almost none:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb21.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image21.png)

This was especially strange because the VM was running over a bridged 100 MBit/s network connection while production has 10 GBit/s connections.

I copied the ThinApp over to my Laptop and ran it from there. Since my laptop is quite a beast (32 GB mem, dual SSD, Intel 2960XM Quadcore with hyperthreading) I expected Office to be fast. The opposite was true: the ThinApp was just as slow as in production.

So what did my laptop and production have in common that the VM didn’t? The answer was multiple cores!

**<u>Multi CPU Bug   
</u>**Since version 2007 Office has was optimized to take advantage of multiple processors. For example, Excel uses [multi-threaded calculation](http://192.168.40.25:8081/2012/06/08/excel-2010-multi-threaded-calculation/) to speedup complex calculation.

Using Multiple Threads is obviously an advantage and can speedup an application or simply keep the UI responsive while performing operations in the background. The downside is that multithreading is vulnerable to race conditions:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb22.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image22.png)A *race condition* occurs when two threads access a shared variable at the same time. The first thread reads the variable, and the second thread reads the same value from the variable. Then the first thread and second thread perform their operations on the value, and they race to see which thread can write the value last to the shared variable. The value of the thread that writes its value last is preserved, because the thread is writing over the value that the previous thread wrote.

Therefore Locking mechanisms are using to prevent race conditions. However locking mechanisms are vulnerable to deadlocking:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb23.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image23.png)A *deadlock* occurs when two threads each lock a different variable at the same time and then try to lock the variable that the other thread already locked. As a result, each thread stops executing and waits for the other thread to release the variable. Because each thread is holding the variable that the other thread wants, nothing occurs, and the threads remain deadlocked.

**<u>Affinity</u>**   
To test if we were bugged by a Multi CPU bug, I opened Task Manager, right clicked on the Outlook process and changed it’s affinity:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb24.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image24.png)

Immediately Outlook started responding nicely and since a natively installed Outlook also worked nicely I could concluded that it was a ThinApp bug.

I filed a support incident with VMware (case # 12211026108) which has not lead to a solution: VMware has closed the ticket 3 times and while discussion is taking place I made a workaround.

**<u>Workaround</u>**   
As you may know, a ThinApp consists of an Entrypoint that loads the Virtual OS (VOS). Once the VOS is running the actual executable is loaded as a child process.

Knowing this we can take advantage of a design choice in Windows: [processor affinity is inherited by child processes](http://blogs.msdn.com/b/oldnewthing/archive/2005/08/17/452630.aspx).

I wrote a tool, named dnkCpuBalancer, that when launched get’s its parent process and modifies the CPU Affinity of that process to a single randomly chosen CPU.

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb25.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image25.png)

This tool is launched inside the ThinApp with a VBS Script using the [OnFirstParentStart](http://blogs.vmware.com/thinapp/2008/10/scripting-withi.html) callback function:

“&gt;

Give the vbs file a name of choice, place it into the ThinApp root folder and rebuild.

Any Process launched from the ThinApp will now run on a single, fixed core. Using Process Explorer we can see how it works:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb26.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image26.png)

When the ThinApp is started first the VOS is launched (Microsoft Outlook 2010.exe with PID 4056). This process launches the actual application, Outlook.exe (PID 1408). Launching Outlook triggers the VBS script that launches dnkCpuBalancer.exe (PID 2128).

dnkCpuBalancer get’s it’s parent process (1408) and modifies it’s affinity to a random CPU. Then dnkCpuBalancer exists itself since it’s no longer needed.

If Outlook will launch any child processes they will automatically inherit the processor affinity from Outlook.exe:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/12/image_thumb27.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/12/image27.png)

**<u>Performance</u>**

There’s no need to worry about performance when tying the process to a fixed CPU. The Windows Kernel will evenly distribute other processes and threads over the available cores taking CPU usage into consideration. As an extra precaution dnkCpuBalancer never selects the first CPU.

**<u>Download</u>**

The dnkCpuBalancer tool is available as [free download](http://www.denamik.com/free-downloads#k) on the Denamik website.