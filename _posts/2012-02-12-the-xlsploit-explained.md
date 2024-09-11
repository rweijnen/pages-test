---
id: 2392
title: 'The XLSploit explained'
date: '2012-02-12T23:36:34+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=2392'
permalink: /2012/02/12/the-xlsploit-explained/
short-url:
    - 'http://bit.ly/Athwaw'
views:
    - '3333'
categories:
    - General
tags:
    - AppSense
    - Excel
    - Exploit
    - RES
---

Recently I published a [Proof of Concept](http://192.168.40.25:8081/2012/01/27/bypassing-res-application-security/) that showed it was possible to launch unauthorized processes with both AppSense Application Manager and RES Workspace Manager.

Although I didn’t test Microsoft Applocker I have no doubt at all that we couldn’t bypass it.

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/02/image_thumb1.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/02/image1.png)I have named my Proof of Concept the **XLSploit** because I am using Excel as a trampoline. I choose Excel because this is generally a trusted process and VBA offers access to the Windows API that is needed.

After publishing the XLSploit I have talked to both [RES](http://www.ressoftware.com/) and [AppSense](http://www.appsense.com/) and not that they both have a response to my Proof of Concept, I consider it safe to tell a little more about how it works.

If you are merely interested in stopping the XLSploit, please scroll down to the end of the article.

**<span style="text-decoration: underline;">How does it work?</span>**  
From a trusted process we launch another copy of any trusted process (the Host Process) using the [CreateProcess](http://msdn.microsoft.com/en-us/library/windows/desktop/ms682425(v=vs.85).aspx) API. In my case I am launching a second instance of Excel from Excel.

In the dwCreationFlags parameter I specify the [CREATE\_SUSPENDED](http://msdn.microsoft.com/en-us/library/windows/desktop/ms684863(v=vs.85).aspx) Process Flag. This will start the process but it will halt after loading ntdll.dll and before executing the Loader Routine. We can see this in Process Explorer, the process is highlighted in gray which indicates it’s in suspended state:

[![SNAGHTML5315eec](http://192.168.40.25:8081/wp-content/uploads/2012/02/SNAGHTML5315eec_thumb.png "SNAGHTML5315eec")](http://192.168.40.25:8081/wp-content/uploads/2012/02/SNAGHTML5315eec.png)

Now we load the process that we actually want to launch (the Target Process), but instead of calling the CreateProcess API we load it in memory as a binary file.

You may have guessed the idea by now: we are going to “replace” the suspended process in memory with the contents we read from disk.

This means we first need to get the address of the [PEB](http://undocumented.ntinternals.net/UserMode/Undocumented%20Functions/NT%20Objects/Process/PEB.html) (Process Environment Block) using the [GetThreadContext](http://msdn.microsoft.com/en-us/library/windows/desktop/ms679362(v=vs.85).aspx) API. The PEB Address is in the EBX field of the retrieved CONTEXT structure.

Using the [ReadProcessMemory](http://msdn.microsoft.com/en-us/library/windows/desktop/ms680553(v=vs.85).aspx) API we read the Image Base Address from the PEB and unmap it from the Host Process.

Using the IMAGE\_DOS\_HEADER, IMAGE\_NT\_HEADERS we can analyze the target process and copy it’s contents to the memory space of the Host Process.

Depending on the OS we need to do some fixups such as base relocations, account for [Side By Side manifests](http://en.wikipedia.org/wiki/Side-by-side_assembly) and MUI resources.

After that we set the Thread Context using the [SetThreadContext](http://msdn.microsoft.com/en-us/library/windows/desktop/ms680632(v=vs.85).aspx) API to the entry point of the Target Process and resume the process using the [ResumeThread](http://msdn.microsoft.com/en-us/library/windows/desktop/ms685086(v=vs.85).aspx) API.

After resuming we seem to have a second instance of Excel.exe:

[![SNAGHTML5482b8d](http://192.168.40.25:8081/wp-content/uploads/2012/02/SNAGHTML5482b8d_thumb.png "SNAGHTML5482b8d")](http://192.168.40.25:8081/wp-content/uploads/2012/02/SNAGHTML5482b8d.png)

Even the Window Title shows Excel:

[![SNAGHTML548baf0](http://192.168.40.25:8081/wp-content/uploads/2012/02/SNAGHTML548baf0_thumb.png "SNAGHTML548baf0")](http://192.168.40.25:8081/wp-content/uploads/2012/02/SNAGHTML548baf0.png)

Even Process Explorer shows that we are running Excel:

[![SNAGHTML5494cd3](http://192.168.40.25:8081/wp-content/uploads/2012/02/SNAGHTML5494cd3_thumb.png "SNAGHTML5494cd3")](http://192.168.40.25:8081/wp-content/uploads/2012/02/SNAGHTML5494cd3.png)

**<span style="text-decoration: underline;">Is it an Excel Exploit?  
</span>**No! We could use any trusted process for this exploit that offers us access to the Windows API. The convenience of VBA in Office is that it allows us full access to the Windows API.

**<span style="text-decoration: underline;">Is it a Windows Exploit?</span>**  
No, I don’t think so. The Windows security model protects you from the outside (think other computers) and from other users. It does not protect you from yourself; you have permissions to open, debug, kill, do anything with your own processes.

**<span style="text-decoration: underline;">Is it malware?  
</span>**The XLSploit is not malware, it doesn’t damage your system in any way. However the code does things that are typically associated with malware. Therefore your Virus Scanner **should** at least find it suspicious. The strange thing is that if I place similar code in an executable or DLL, all Virus Scanners alert. But when used from VBA most Virus Scanners seem to allow it.

**<span style="text-decoration: underline;">What it does not do  
</span>**Using this method we can successfully launch unauthorized processes, however we do not gain additional privileges (aka [privilege escalation](http://en.wikipedia.org/wiki/Privilege_escalation)). If you are a standard user, you still will not have permissions to delete system folders and files. It could of course be a first step towards that but we would need other exploits for it.

**<span style="text-decoration: underline;">How to stop this Exploit?  
</span>**First of all: you should set [Office Macro Security](http://office.microsoft.com/en-us/office-2003-resource-kit/macro-security-levels-in-office-2003-HA001140307.aspx) to High and prevent users from changing it. I know this probably breaks all applications you use, so you should really put pressure on your vendors and have them sign their macro’s. Or as an alternative, [self sign them](http://192.168.40.25:8081/2011/01/12/self-signing-word-macros/).

Secondly: Implement A Virus Scanner that uses the [Office AntiVirus API](http://msdn.microsoft.com/en-us/library/ie/ms537369(v=vs.85).aspx) to scan documents and macro’s. Most Home versions of AV products seem to have this while Enterprise products often do not. Perhaps because of performance reasons.

For RES Workspace Manager see: [Q203322 HOWTO: How to prevent malicious Microsoft Office macros](http://support.ressoftware.com/Modules/KnowledgeBase/knowledgebaseTreeView.aspx?id=3322) and [here](http://youtu.be/WnWVMQPyL8U).

For AppSense Application Manager see: [AppSense Blog](http://www.appsense.com/blog/post/2012/02/11/VBA-Exploit-AppSense-Application-Manager.aspx)