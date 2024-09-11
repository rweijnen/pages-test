---
id: 1448
title: 'Windows 2003 Server Standard memory patch'
date: '2011-03-27T21:42:17+02:00'
author: Oliver
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1448'
permalink: /2011/03/27/windows-2003-server-standard-memory-patch/
short-url:
    - 'http://bit.ly/eFYsJ0'
views:
    - '10904'
categories:
    - 'Windows 2003'
    - 'Windows Internals'
---

So a few days ago I got new memory for a development box – an upgrade from 4 to 6 GiB (later on even 8 GiB). Much appreciated as you can imagine. After dismissing the BIOS warning about changed amount of memory (oh really? :mrgreen:), I booted into Ubuntu and happily looked at the memory stats. After that I booted into Windows (a Windows 2003 Server Standard, but I’ll just use Windows from here on) and was disappointed to see only 4 GiB available. This is apparently a limitation specific to the Standard edition.

After some pouting, I decided to take action. Of course one of my first thoughts was to ask Remko, because he had done similar things for some other Windows versions. He pointed me to `MmInitSystem`, which was not an immediate hit, though. I loaded my kernel .exe into a disassembler to look at the details, but `MmInitSystem` was a lengthy and rather boring function. However, the advice was good and got me a good bit closer, especially when Remko also mentioned the use of `ExVerifySuite` in the logic that would set the limits. So I brought up the references to `ExVerifySuite` and – surprise surprise – only seven other functions used it and out of these only one was not recognized by name from the exports and debug symbols. And since the inspection of that function (at `0x00615FB0` in my kernel) proved that it was being called from `MmInitSystem`, this was an immediate hit.

Now the only problem was to figure out the logic of that function. Remko gave me a rough idea here as well. Since I was too lazy to map his findings to my binary, I decided to simply find out which variable held the limitation and fix *all* instances instead of the one pertaining to my specific edition ![:mrgreen:](http://192.168.40.25:8081/wp-includes/images/smilies/mrgreen.png)

The limits in the kernel are expressed in number of pages, instead of bytes or similar. Hence all values in that function in question had to be multiplied by 0x1000, i.e. the default page size. The two 32bit variables at the bottom of the stack (within that function) turned out to be those limits. The code passes some value to [`ExVerifySuite`](http://www.geoffchappell.com/studies/windows/km/ntoskrnl/api/ex/exinit/productsuite.htm) several times, obviously expecting either true (1) or false (0) in return. Since my lazy approach didn’t require the deeper understanding, I made a mental notice (after all Geoff Chappell’s site is well known among reversers) about the location where to find the details and proceeded. The apparent maximum for these two variables within the function was 0x10000000 I decided to replace all instances with that value. For the 32bit second value (from stack bottom) this meant replacing a  
“&gt;  
… at the beginning of the function and several  
“&gt;  
… by either populating the assigned register (e.g. eax) correctly on the right side or assigning the proper immediate value.

All in all I patched 16 bytes. It may seem excessive for the job at hand, but I couldn’t help my laziness and thus deemed it cheaper to invest mere seconds in the repetitive task of patching all occurrences rather than understanding the logic entirely within some minutes.

Okay, that work was done, so I thought I would get away with simply placing an additional line in `boot.ini` and load the patched kernel. If something went wrong I still had the Linux installation for recovery. So I simply copied the existing line in `boot.ini` and appended:  
“&gt;  
One reboot later I knew something was missing. Sure enough Remko was spot on when he asked me whether I had corrected the PE image checksum after the patch. Well, I hadn’t. He also pointed me to one tool that I hadn’t used for this purpose before:  
“&gt;  
… fixed the image checksum and the next reboot proved that it worked.

As mentioned at the beginning, meanwhile I have two more GiB of RAM and the screenshots attached to this post prove that the patch works just fine (removed some details from the “General” tab, though).

The [dUP2](http://192.168.40.25:8081/2011/03/27/dup2-patcher-update/) file can be downloaded below but before you continue please ensure that patching the kernel is allowed according to your country’s laws and your license agreement.

[ Windows 2003 Standard Kernel Patch (3047 downloads ) ](http://192.168.40.25:8081/download/windows-2003-standard-kernel-patch/?tmstv=1726048919 "Version 1")

If you don’t know how to use dUP2, please read [Universal Patcher](http://192.168.40.25:8081/2008/12/09/new-universal-patch-method/)

// Oliver

 <style type="text/css">
			#gallery-1 {
				margin: auto;
			}
			#gallery-1 .gallery-item {
				float: left;
				margin-top: 10px;
				text-align: center;
				width: 33%;
			}
			#gallery-1 img {
				border: 2px solid #cfcfcf;
			}
			#gallery-1 .gallery-caption {
				margin-left: 0;
			}
			/* see gallery_shortcode() in wp-includes/media.php */
		</style>

<div class="gallery galleryid-1448 gallery-columns-3 gallery-size-thumbnail" id="gallery-1"><dl class="gallery-item"> <dt class="gallery-icon portrait"> [![](http://192.168.40.25:8081/wp-content/uploads/2011/02/taskmgr-150x150.png)](http://192.168.40.25:8081/wp-content/uploads/2011/02/taskmgr.png) </dt></dl><dl class="gallery-item"> <dt class="gallery-icon portrait"> [![](http://192.168.40.25:8081/wp-content/uploads/2011/02/8gib-150x150.png)](http://192.168.40.25:8081/wp-content/uploads/2011/02/8gib.png) </dt></dl>   
 </div>