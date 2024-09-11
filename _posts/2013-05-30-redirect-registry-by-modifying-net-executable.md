---
id: 3284
title: 'Redirect Registry by Modifying .NET Executable'
date: '2013-05-30T16:18:23+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3284'
permalink: /2013/05/30/redirect-registry-by-modifying-net-executable/
short-url:
    - 'http://bit.ly/ZgBn2C'
views:
    - '1980'
categories:
    - 'Application Compatibility'
    - Citrix
tags:
    - .NET
    - AppCompat
    - Citrix
    - Reflector
    - XenApp
---

Yesterday I was troubleshooting an application that was migrated to Citrix XenApp.

The application is able to use a high precision scale which is attached to the client pc’s com port. This com port is redirected to XenApp.

While testing users reported several issues, let’s have a look at them.

**<u>Error configuring COM Port</u>**   
Within the application the comport to which the scale is connected must be configured:

[![De compoort lezer staat uit](http://192.168.40.25:8081/wp-content/uploads/2013/05/image_thumb3.png "Compoort instellen")](http://192.168.40.25:8081/wp-content/uploads/2013/05/image3.png)

After pressing "Registreer" to register the new com port the following error message was shown

[![Er staat geen compoort in het register. Registreer eerste de juiste compoort](http://192.168.40.25:8081/wp-content/uploads/2013/05/image_thumb4.png "Fout")](http://192.168.40.25:8081/wp-content/uploads/2013/05/image4.png)

A Process Monitor trace quickly revealled the problem, the application wants to store the configuration in the HKLM hive:

[![Process Monitor Trace](http://192.168.40.25:8081/wp-content/uploads/2013/05/image_thumb5.png "Process Monitor")](http://192.168.40.25:8081/wp-content/uploads/2013/05/image5.png)

We could circumvent this error by pre-creating this registry key and adding Modify permissions to it. However this presents us with a new issue in multi-user environments such as XenApp: *if one user actives this setting, it is activated for all users on the same server*.

**<u>Registry Redirection</u>**   
That’s why I figured that redirecting this particular registry key from HKLM to HKCU would be the best solution.

I first tried the [VirtualRegistry](http://technet.microsoft.com/en-us/library/cc749368%28v=ws.10%29.aspx) but even though AcLayers.dll was on stack right before the call to RegCreateKeyExW the redirection didn’t work.

Time to bring in other tools!

**<u>Disassemble</u>**   
Regular readers probably expect me to launch Ida Pro now but I saw that mscorwks.dll was on stack in the Process Monitor trace. This means that we are dealing with a .NET application for which [.NET Reflector](http://www.red-gate.com/products/dotnet-development/reflector/) is a better choice.

But before starting Reflector I needed to know what file(s) I needed to analyze. I used Total Commander, one of my favourite tools, to search for the word TransponderLezer since this is the registry value that contains the com port number.

I got no hits in the application’s program folder so I repeated the search but this time searching for [Unicode](http://en.wikipedia.org/wiki/Unicode). Again no hits, so I repeated the search on the .NET Assembly folder (C:\\Windows\\Assembly) and with Unicode I had a hit:

[![Total Command | Find Files Dialog](http://192.168.40.25:8081/wp-content/uploads/2013/05/image_thumb6.png "Find Files")](http://192.168.40.25:8081/wp-content/uploads/2013/05/image6.png)

I loaded the file "ChipSoft.Ezis.Steriel.dll" in Reflector and expanded it. Under References I noticed Borland.Delphi which tells me the application is written in Delphi.NET. Therefore I set the language in Reflector to Delphi:

[![.NET Reflector 6](http://192.168.40.25:8081/wp-content/uploads/2013/05/image_thumb7.png ".NET Reflector 6")](http://192.168.40.25:8081/wp-content/uploads/2013/05/image7.png)

Note that choosing the same language as the application was built in is not required. Since .NET code is compiled to intermediate code (IL) you could also select C# or VB.NET. However I think that selecting the original language might make it easier to read code that calls into the language’s [RTL](http://en.wikipedia.org/wiki/Runtime_library).

I enabled Search via the View Menu and selected the Search String or Constant option:

[![image](http://192.168.40.25:8081/wp-content/uploads/2013/05/image_thumb8.png "image")](http://192.168.40.25:8081/wp-content/uploads/2013/05/image8.png)

I searched for TransponderLezer again and that yielded a couple of results:

[![.NET Reflector | Search](http://192.168.40.25:8081/wp-content/uploads/2013/05/image_thumb9.png "Search")](http://192.168.40.25:8081/wp-content/uploads/2013/05/image9.png)

I opened the one called btnSchrijfKeyInRegisterClick (Write Key to Registry) and watched the Disassembly output:

 “&gt;

This procedure calls into a function named schrijfRegisterSleutelString. Let’s have a look at that one:

“&gt;

The interesting part is the marked line, the value -2147483646 is passed as the RootKey. The corresponding hex value is 80000002 which is the same as the constant value for HKEY\_LOCAL\_MACHINE:

“&gt;

So if we edit this value to the constant value of HKEY\_CURRENT\_USER we have redirected the registry key from HKLM to HKCU.

**<u>Modifying the binary</u>**

To do this we need to download an Add-In for Reflector called [Reflexil](http://reflexil.net/). To activate it, go to View | Add-Ins:

[![Reflector | Add-Ins](http://192.168.40.25:8081/wp-content/uploads/2013/05/image_thumb10.png "Add-Ins")](http://192.168.40.25:8081/wp-content/uploads/2013/05/image10.png)

After adding it, choose Tools | Reflexil 1.6 to open Reflexil’s view. Select the line you want to edit, in my case line 06.

[![Reflexil Add-In for Reflector](http://192.168.40.25:8081/wp-content/uploads/2013/05/image_thumb11.png "Reflexil")](http://192.168.40.25:8081/wp-content/uploads/2013/05/image11.png)

Right-click and choose Edit. The new value is the Decimal value of the HKEY\_CURRENT\_USER constant which is -2147483647:

[![Modify .NET intermediate code](http://192.168.40.25:8081/wp-content/uploads/2013/05/image_thumb12.png "Edit existing instruction")](http://192.168.40.25:8081/wp-content/uploads/2013/05/image12.png)

Press update and make the same change to the read function which is called *leesRegisterSleutelString*.

It’s a good idea to check if other parts of the code are calling into these functions. You can do this by Right-clicking on the function in the left tree view and selecting Analyze.

In the Analyzer expand the "Used By" node, in my case only code related to the com port is affected. Therefore I consider the change to be safe:

[![Reflector | Analyzer | Used By | Dependancies](http://192.168.40.25:8081/wp-content/uploads/2013/05/image_thumb13.png "Analyzer")](http://192.168.40.25:8081/wp-content/uploads/2013/05/image13.png)

**<u>Committing changes</u>**

Finally we can save the modified executable to disk by Right-clicking on the file in the left treeview and selecting Reflexil | Save As.

**<u>Fixed?</u>**

Usually I end my blogs with a sentence like: and that fixed it!

However now that the scale is working, testers have reported issues when they roam their XenApp session to another pc.

If the application is connected to the scale and the session is reconnected to a client pc without scale the application presents an error message which is acceptable.

But when you later on reconnect to a client with an attached scale the application crashes.

So in the end this fix did not go into production, not even as temporary workaround. We are going to contact the vendor and request a fix for both issues.