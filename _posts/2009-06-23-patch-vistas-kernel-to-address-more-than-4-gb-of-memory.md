---
id: 379
title: 'Patch Vista&#8217;s Kernel to Address more than 4 GB of Memory'
date: '2009-06-23T16:33:03+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2009/06/23/patch-vistas-kernel-to-address-more-than-4-gb-of-memory/'
permalink: /2009/06/23/patch-vistas-kernel-to-address-more-than-4-gb-of-memory/
views:
    - '82791'
short-url:
    - 'http://bit.ly/ex5gvL'
categories:
    - General
    - Vista
---

As you may know the 32 bit, also called x86, editions of Windows Vista cannot address more than 4 GB of memory. You may think this 4 GB is a limit of the processor but this isn’t true; using Physical Address Extension (PAE) it’s possible to address more memory

Enterprise Server versions of Windows (2003 and 2008) can already address more than 4 GB of memory so why can we not do that with Vista? The answer is: Microsoft doesn’t want that! It is all just a licensing matter, we can see this in the registry. Take RegEdit and goto HKLM\\CurrentControlSet\\Control\\ProductOptions and doubleclick the Value ProductPolicy, scroll down a little until you see the value “Kernel-PhysicalMemoryAllowedx86”, next to it is the value 01 00 which corresponds to 4096 (1000 is the Hex of 4096):

[![RegEdit1](http://192.168.40.25:8081/wp-content/uploads/2009/06/regedit1-2-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/06/regedit1-2.png)

We can also see where this (and other licensing values) come from: if we look in %systemroot%\\system32\\licensing\\ppdlic al license values are in XML files. The memory value is in Kernel-ppdlic.xrm-ms:

[![LicenseXML](http://192.168.40.25:8081/wp-content/uploads/2009/06/licensexml-1-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/06/licensexml-1.png)

We cannot make alterations to the registry values or to this xml file; the registry values are rebuild on every reboot and the xml file is protected with a signature

Unlike other licensing values (like activation) which are checked through slc.dll this particular value is checked and enforced in the Kernel. I though that patching the kernel was not possible due to Vista’s integrity checks. But then I read Geoff Chappell’s [article](http://www.geoffchappell.com/viewer.htm?doc=notes/windows/license/memory.htm) about Vista’s Memory Limit. Geoff describes in detail how and where the check is done and even shows us what to patch.

I followed Geoff’s description and patched my kernel (carefully read Geoff’s instruction about checksum, signing and so on!) and I can confirm that his patch works perfectly!

I also noticed something else: my Dell Laptop has 4 GB of memory of which I was only able to use about 3,5 GB. This is due to a portion of the address space that is allocated to my video card (address space, not memory!). We can see with [Alex Ionescu’s](http://www.alex-ionescu.com/) [MemInfo tool](http://www.winsiderss.com/tools/meminfo/meminfo.htm) there’s a gap between 9F0000 and 100000:

[![MemInfo1](http://192.168.40.25:8081/wp-content/uploads/2009/06/meminfo1-1-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/06/meminfo1-1.png)

We can also see in TaskManager that only 3581 MB is available:

[![TaskMgr1](http://192.168.40.25:8081/wp-content/uploads/2009/06/taskmgr1-1-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/06/taskmgr1-1.png)

Let’s see what that looks like after the patch:

[![meminfo2](http://192.168.40.25:8081/wp-content/uploads/2009/06/meminfo2-1-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/06/meminfo2-1.png)

We gained the missing memory 😉

TaskManager confirms that:

[![taskmgr2](http://192.168.40.25:8081/wp-content/uploads/2009/06/taskmgr2-1-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/06/taskmgr2-1.png)

If you would like to patch your kernel you can download the [dUP2](http://192.168.40.25:8081/2008/12/09/new-universal-patch-method/) file below. Please check if creating and/or using this patch is legal according to your country’s laws and your license agreement and of course carefully read [Geoff’s instructions](http://www.geoffchappell.com/viewer.htm?doc=notes/windows/license/memory.htm).

The patch was tested on both SP1 and SP2.

[ Vista NT Kernel Patch (11573 downloads ) ](http://192.168.40.25:8081/download/vista-nt-kernel-patch/?tmstv=1726048918 "Version 1.0")