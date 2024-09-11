---
id: 2242
title: 'Citrix online plug-in received a corrupt ICA File'
date: '2011-12-13T10:58:24+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/12/13/citrix-online-plug-in-received-a-corrupt-ica-file/'
permalink: /2011/12/13/citrix-online-plug-in-received-a-corrupt-ica-file/
views:
    - '14126'
short-url:
    - 'http://bit.ly/u837iE'
categories:
    - Citrix
    - script
---

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/12/image_thumb4.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/12/image4.png)I was testing a Script I wrote to launch a Citrix XenApp session using the Ica Client Object. Typical code to do this may look like this:

 “&gt;

On my testmachine it ran nicely but on a customer machine the script failed with the error 2312 “*The Citrix online plug-in received a corrupt ICA File. The ICA File has no \[ApplicationServer\] section*“:

[![The Citrix online plug-in received a corrupt ICA File. The ICA File has no [ApplicationServer] section](http://192.168.40.25:8081/wp-content/uploads/2011/12/SNAGHTML108b7fef_thumb.png "Error number 2312")](http://192.168.40.25:8081/wp-content/uploads/2011/12/SNAGHTML108b7fef.png)

I couldn’t find any errors in my script so I fired up Process Monitor and noticed that the Ica Client Object creates a temporary .ica file in the %temp% folder. When it tried to write to this file this fails because access is denied:

[![Process Monitor | ACCESS DENIED | temp folder | temporary ICA file](http://192.168.40.25:8081/wp-content/uploads/2011/12/SNAGHTML10912948_thumb.png "Process Monitor")](http://192.168.40.25:8081/wp-content/uploads/2011/12/SNAGHTML10912948.png)

I Checked the temporary .ica file and it was empty (0 bytes). Then I used the Stack View option from Process Monitor on the first ACCESS DENIED event:

[![Process Monitor | Stack View](http://192.168.40.25:8081/wp-content/uploads/2011/12/image_thumb5.png "Event Properties")](http://192.168.40.25:8081/wp-content/uploads/2011/12/image5.png)

From the stack we can see that fltMgr.sys is the last to touch the file (stack is from bottom to top). fltMgr is the File System Filter Driver which makes it likely that the Virus Scanner is blocking access. So I checked the Anti Virus log, McAfee VirusScan in my case:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/12/image_thumb6.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/12/image6.png)

The text is in Dutch, it says: Blocked by the access rule: maximum Anti-Spyware: prevent script execution from the temp folder.

So McAfee considers the .ica a script since it’s created by the process cscript.exe.

Unfortunately the Ica Client Object doesn’t offer a method or property to change the folder where the temporary ica file is created. I decided to have look at Wfica.ocx with Ida Pro and noticed that the [GetTempPath](http://msdn.microsoft.com/en-us/library/windows/desktop/aa364992(v=vs.85).aspx) and [GetTempFilename](http://msdn.microsoft.com/en-us/library/windows/desktop/aa364991(v=VS.85).aspx) API’s are used to assemble the filepath.

In the remarks section of the GetTempPath documentation on MSDN states that it looks first to the %TMP% environment variable.

So we can easily workaround this issue by changing the %TMP% variable before we run our script:

“&gt;