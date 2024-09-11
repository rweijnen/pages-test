---
id: 135
title: 'Switch between Java Versions'
date: '2008-11-12T16:37:52+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2008/11/12/switch-between-java-versions/'
permalink: /2008/11/12/switch-between-java-versions/
views:
    - '20708'
short-url:
    - 'http://bit.ly/f9WKfH'
categories:
    - Java
    - Uncategorized
tags:
    - Business
    - Java
    - jre
    - jvm
    - Objects
    - Switch
    - Versions
---

On a Citrix environment 2 different applications were required. One of the applications required java version 1.5 (and didn’t work with 1.6) and the other application needed specifically version 1.6.

Because the applications are installed on a Citrix server the users do not have write permissions to HKEY\_LOCAL\_MACHINE so that was another complication.

After a lot of monitoring with process monitor the general process of how a particular Java version loads in Internet Explorer became clear to me.

Java adds an addon to IE called ssv.dll, you can see this trough Tools | Manage Add-ons | Enable or Disable Add-ons:

[![AddOns](http://192.168.40.25:8081/wp-content/uploads/2008/11/addons-3-small.png)](http://192.168.40.25:8081/wp-content/uploads/2008/11/addons-3.png)

In Internet Explorer there is a setting under Tools | Internet Options | Advanced | Java (Sun). This activates Sun Java instead of the Microsoft version:

![UseJre](http://192.168.40.25:8081/wp-content/uploads/2008/11/usejre-1.png)

This setting corresponds to a registry key:

[![JAVA SUN](http://192.168.40.25:8081/wp-content/uploads/2008/11/java-sun-2-small.png)](http://192.168.40.25:8081/wp-content/uploads/2008/11/java-sun-2.png)

This setting (when activated) sets the UseJava2Iexplorer value under the path specified in the RegPath key:

[![UseJava2Iexplorer](http://192.168.40.25:8081/wp-content/uploads/2008/11/usejava2iexplorer-1-small.png)](http://192.168.40.25:8081/wp-content/uploads/2008/11/usejava2iexplorer-1.png)

So my first try was to change the RegPath value to Software\\JavaSoft\\Java Plug-in\\1.5.0\_11 but that doesn’t work. Problem is that IE has already loaded the Add-on ssv.dll from version 1.6 and we now make the registry point to 1.5. So we need to load the Add-on from version 1.5 as well.

So next question: where is this add-in loaded?

I searched in the registry for ssv.dll and found it in several places:

> \[HKEY\_CLASSES\_ROOT\\CLSID\\{5852F5ED-8BF4-11D4-A245-0080C6F74284}\\InprocServer32\]  
> @=”C:\\\\Program Files\\\\Java\\\\jre1.5.0\_11\\\\bin\\\\JavaWebStart.dll”  
> “ThreadingModel”=”Apartment”
> 
> \[HKEY\_CLASSES\_ROOT\\CLSID\\{761497BB-D6F0-462C-B6EB-D4DAF1D92D43}\\InprocServer32\]  
> @=”C:\\\\Program Files\\\\Java\\\\jre1.5.0\_11\\\\bin\\\\ssv.dll”  
> “ThreadingModel”= “Apartment”

It’s interesting to know that HKEY\_CLASSES\_ROOT is actually a merged view of HKEY\_CURRENT\_USER\\Software\\Classes and HKEY\_LOCAL\_MACHINE\\Software\\Classes. In this case the actual values were in the HKLM branch. So I began experimenting by adding the same keys in HKCU and modifying it’s values.

After some experimenting I came up with 2 reg files that can be imported that contain only HKEY\_CURRENT\_USER keys. Maybe these reg files can be tweaked further but I verified the correct working by Java version check on <http://www.javatester.org/version.html>.

So simply importing a reg file can switch between Java versions! In the production environment I will make a particular version default based upon Group membership and/or the application that is started. The actual change will be done by script and maybe the user will be offered a nice “switch java version” tool. But the principle is ofcourse to make the proper registry entries under HKCU.

You can download the registry files below, please note that I used Java versions 1.5.011 and 1.6.07. So the reg files are based on those versions!

[ Java Version Switch Registry Files (4186 downloads ) ](http://192.168.40.25:8081/download/java-version-switch-registry-files/?tmstv=1726048918 "Version 1.0")

PS Sun offers older versions download on a special [page](http://java.sun.com/products/archive/ "Archive: Java[tm] Technology Products Download").