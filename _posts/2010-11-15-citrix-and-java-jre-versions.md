---
id: 775
title: 'Citrix and Java JRE Versions'
date: '2010-11-15T22:41:33+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=775'
permalink: /2010/11/15/citrix-and-java-jre-versions/
views:
    - '6783'
short-url:
    - 'http://bit.ly/gdJ9vR'
categories:
    - Citrix
    - Java
    - Packaging
    - 'Unattended Installation'
---

If you have ever installed Citrix Presentation Server/XenApp or one of the management consoles then you have probably dealt with Java versions.

Citrix is very picky about the Java version so it’s usually best to initially install the Jre version that is delivered with the product.

In my case however I needed to install the CMC for Xenapp 5 on Windows 2003, it requires JRE 5.0 Update 9 but this version was undesirable.

So I tried to install the CMC with the current JRE version (1.6.0\_22 at this time) but it makes the Installer exit immediately:

![CMCJreError](http://192.168.40.25:8081/wp-content/uploads/2010/11/cmcjreerror.png)

We can bypass this check by creating a Transform and modifying a few Public Properties (I tried to set the values for these properties by commandline but this is not accepted).

I used the Orca tool from the Windows SDK, here are the steps required:

1. Start Orca and open the CMC.msi
2. <div>From the Menu Bar select Transfrom | New Transform</div>
3. <div>Select the Property table and sort the Properties by their name:</div>

[![Orca1](http://192.168.40.25:8081/wp-content/uploads/2010/11/orca1-1-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/11/orca1-1.png)

Now modify the following properties:

Accept from No -&gt; Yes  
CTX\_USE\_EXISTING\_JRE from No -&gt; Yes  
JRERequiredVersion.E345A43A\_38FF\_4EEE\_AF87\_24C4B9518A97 from 1.5.0\_09 -&gt; 1.6

1. Choose Transfrom | Generate Transfrom from the Menu Bar
2. Save the transfrom
3. Launch MsiExec with the TRANSFORMS option, eg MsiExec /i CMC.msi TRANSFORMS=Java16.mst

Now you can install the CMC with any Java 1.6xxx version.

But we’re not finished yet!

When I attempted to start the console after install I got this error:

[![CMCJreError2](http://192.168.40.25:8081/wp-content/uploads/2010/11/cmcjreerror2-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/11/cmcjreerror2.png)

This error is caused by the console loader, a small executable called CtxLoad.exe which resides in %ProgramFiles%\\Citrix\\Administration (or %ProgramFiles(x86)% on x64 systems).

But why does it fail? To find out I opened CtxLoad.exe in Ida Pro and searched for strings containing Java:

[![CtxLoad1](http://192.168.40.25:8081/wp-content/uploads/2010/11/ctxload1-1-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/11/ctxload1-1.png)

That looks like it’s a hardcoded path! In Ida you can use Ctrl-X to check for references and the Disassembly shows the registry check:

[![CtxLoad2](http://192.168.40.25:8081/wp-content/uploads/2010/11/ctxload2-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/11/ctxload2.png)

In C that would like like:  
“&gt;  
So now we can fix it, there are several possible solutions:

1. <div>Create fake 1.5 Java registry keys pointing to the same values as 1.6.</div>
2. <div>Create a REG_LINK (it’s possible to create shortcuts in the registry) for Java 1.5 pointing to 1.6</div>
3. <div>Patch CtxLoad.exe</div>

I choose 3 since I think 1 is a bad idea and 3 is done a lot faster than 2.

For you convenience you can download all files: [ CtxLoad Patch (2005 downloads ) ](http://192.168.40.25:8081/download/ctxload-patch/?tmstv=1726048919 "Version 1")

The download includes the original and the patched CtxLoad, the patcher with dUP2 source and the mst file.