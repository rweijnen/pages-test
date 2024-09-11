---
id: 123
title: 'RDP Session with Local Taskbar visible'
date: '2008-11-02T08:16:42+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2008/11/02/rdp-session-with-local-taskbar-visible/'
permalink: /2008/11/02/rdp-session-with-local-taskbar-visible/
views:
    - '11962'
short-url:
    - 'http://bit.ly/gDmx96'
categories:
    - 'Terminal Server'
    - Vista
---

I usually have lots of Terminal Server sessions open when I’m working, both direct sessions but also “sessions in sessions”. In order to keep overview on my desktop I prefer to make the session size as big as possible without being full screen (so keep my local taskbar visible).

![LocalTaskBar2](http://192.168.40.25:8081/wp-content/uploads/2008/11/localtaskbar2.png)

If I run a session in a session I do this again, this makes switching sessions very easy and you can always see which session you are in:

![LocalTaskBar3](http://192.168.40.25:8081/wp-content/uploads/2008/11/localtaskbar3-1.png)

In order to specify the desktop size to mstsc we have to know the height of the taskbar and subtract this from the height of the desktop. Because the taskbar can be made bigger or it’s size can be different when a theme (such as Vista’s AERO is used) I wrote a little tool that calculates the correct desktop size and kicks off the session. I used the SystemParametersInfo and GetSystemMetrics API’s:

> SystemParametersInfo(SPI\_GETWORKAREA, 0, @Rect, 0)
> 
> // Width = Screen Width  
> ScrWidth := Rect.Right – Rect.Left;  
> // Height = Maximum client area height  
> ScrHeight := GetSystemMetrics(SM\_CYFULLSCREEN);

Next I run mstsc specifying the width and height and the rdp file as given by commandline:

> if (UpperCase(ParamStr(1)) = ‘/CONSOLE’) or (UpperCase(ParamStr(1)) = ‘/ADMIN’) then  
> begin  
> sCmd := Format(‘%s\\system32\\mstsc.exe “%s” /w:%d /h:%d %s %s’,  
> \[WinDir, ParamStr(2), ScrWidth, ScrHeight, ParamStr(1), ParamStr(3)\]);  
> end  
> else begin  
> sCmd := Format(‘%s\\system32\\mstsc.exe “%s” /w:%d /h:%d %s %s’,  
> \[WinDir, ParamStr(1), ScrWidth, ScrHeight, ParamStr(2), ParamStr(3)\]);  
> end;

Finally I add my tool to the registry so it appears in the context menu of RDP Files.

![Menu](http://192.168.40.25:8081/wp-content/uploads/2008/11/menu.png)

You can find the Tool and .reg file in this download: [ RDPWithLocalTaskbar (3321 downloads ) ](http://192.168.40.25:8081/download/rdpwithlocaltaskbar/?tmstv=1726048918 "Version 1.0")

PS the reg file was made for the v6 version of the MSTSC client where the /console switch was replaced /admin. If you have an older version you need to modify HKEY\_CLASSES\_ROOT\\RDP.File\\shell\\connectconsole\\command

> “%systemroot%\\system32\\RDPWithLocalTaskbar.exe” “%1” /admin

to

> “%systemroot%\\system32\\RDPWithLocalTaskbar.exe” “%1” /console