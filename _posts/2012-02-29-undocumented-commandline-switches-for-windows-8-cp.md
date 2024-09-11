---
id: 2483
title: 'Undocumented commandline switches for Windows 8 CP'
date: '2012-02-29T23:29:18+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2012/02/29/undocumented-commandline-switches-for-windows-8-cp/'
permalink: /2012/02/29/undocumented-commandline-switches-for-windows-8-cp/
views:
    - '6124'
short-url:
    - 'http://bit.ly/yqWMii'
categories:
    - 'Windows 8'
tags:
    - bug
    - Debugger
    - 'Windows 8'
---

The Windows 8 Consumer Preview is downloaded as a Web Installer called Windows8-ConsumerPreview-setup.exe.

On my system the Web Installer crashed while checking Application Compatibility:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/02/image_thumb22.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/02/image22.png)

I clicked the Debug option and launched the Visual Studio debugger:

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/02/image_thumb23.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/02/image23.png)

Websetup crashed in Wica.dll (Windows Install Compability Advisor) because eax is null (smells like a bug), so I wanted to do some more analysis. Wica.dll comes bundled with the Web Setup and is extracted along with the other bundled files into the temp folder (in my case %temp%\\1fd52b5b-2609-4156-ac02-49dca27a0a8d\\WebSetupExpanded).

In the WebSetupExpanded folder is an executable called WebSetup.exe but when we launch it directly we get an error:

[![SNAGHTML65cd3f](http://192.168.40.25:8081/wp-content/uploads/2012/02/SNAGHTML65cd3f_thumb.png "SNAGHTML65cd3f")](http://192.168.40.25:8081/wp-content/uploads/2012/02/SNAGHTML65cd3f.png)

I figured we needed the pass some argument on the commandline to run it directly, so I loaded Websetup.exe in Ida Pro. Websetup parses it’s commandline in an internal function called ConX::Setup::Web::CWebSetupCommandLineInfo::ParseParam

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/02/image_thumb24.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/02/image24.png)

In the screenshots we can see that the following commandline arguments are being checked:

- <font color="#35383d">main</font>
- <font color="#35383d">late</font>
- <font color="#35383d">showerr</font>
- <font color="#35383d">install</font>
- <font color="#35383d">elevate</font>
- <font color="#35383d">silent</font>

So let’s see what they do:

**/main** is required to start without showing the launch error.

**/late** shows a dialog to enter the product id:

[![SNAGHTML6b1305](http://192.168.40.25:8081/wp-content/uploads/2012/02/SNAGHTML6b1305_thumb.png "SNAGHTML6b1305")](http://192.168.40.25:8081/wp-content/uploads/2012/02/SNAGHTML6b1305.png)

**/showerr** shows a dialog indicating your PC doesn’t meet system requirements:

[![SNAGHTML6cc6d9](http://192.168.40.25:8081/wp-content/uploads/2012/02/SNAGHTML6cc6d9_thumb.png "SNAGHTML6cc6d9")](http://192.168.40.25:8081/wp-content/uploads/2012/02/SNAGHTML6cc6d9.png)

**/elevate** and **/silent** are meant to be used together with the other switches.

So in order to run the WebSetup from the debugger I needed the /main argument.

Very nicely: Because I ran the debugger I noticed that Websetup outputs debug info:

 “&gt;

From this debug output I could see that is calls WicaInventory.exe and writes a log file and an XML file into %AppData”%\\Local\\Microsoft\\WebSetup\\Panther\\”.

Interesting to see what’s in there.

But finally we get to the point where it crashes:

\[ConX::Compatibility::Wica::GetDeviceList\] Loading list of devices.

The instruction at 0xFEE49A8 referenced memory at 0x0. The memory could not be read -&gt; 00000000 (exc.code c0000005, tid 8668)

From the disassembly we can see it’s a bug, at function start is the instruction xor edi, edi (after which edi is 0) after that follows and \[ebp+var\_240\] which makes that variable 0 (var\_240 equals ebp (the stack pointer) – 240h which you saw in the Visual Studio debugger screenshot):

[![image](http://192.168.40.25:8081/wp-content/uploads/2012/02/image_thumb25.png "image")](http://192.168.40.25:8081/wp-content/uploads/2012/02/image25.png)