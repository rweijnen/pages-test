---
id: 224
title: 'Windows XP x64 Terminal Server Patch part 2 (optional)'
date: '2009-01-16T00:32:05+01:00'
author: daNIL
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2009/01/16/windows-xp-x64-terminal-server-patch-part-2-optional/'
permalink: /2009/01/16/windows-xp-x64-terminal-server-patch-part-2-optional/
views:
    - '15291'
short-url:
    - 'http://bit.ly/eM1Et6'
categories:
    - General
    - 'Terminal Server'
---

In [part 1](/blog/2008/12/19/windows-xp-x64-terminal-server-patch-part-1-mandatory/) I’ve showed how to get rid of some terminal server restrictions on Windows xp x64. But there are still some problems:

1\) You cannot connect to the <font color="#0080ff">localhost</font> (<font color="#0080ff">127.0.0.1</font>) (but can to *127.a.b.c*, where a,b,c in \[0..255\] (except <font color="#0080ff">127.0.0.0</font> and <font color="#0080ff">127.255.255.255</font>)).

When you’re connecting to remote server, Remote Desktop Connection (mstsc.exe) checks through mtscax.dll that you’re connecting to your own address, connections are only allowed and you’re in the server mode. If this is not true, the connection is denied, usually with this message: [![ConsoleFailed](http://192.168.40.25:8081/wp-content/uploads/2009/01/consolefailed-small.gif)](http://192.168.40.25:8081/wp-content/uploads/2009/01/consolefailed.gif). The logic of checking is the same: call <font color="blue">gethostbyname</font> for server name and check if it’s not equal to <font color="#0080ff">127.0.0.1</font>. So, we can use a very simple patch: find and replace <font color="#ff8040">7F 00 00 01</font> (<font color="#0080ff">127.0.0.1</font>) with, for example, some invalid network address, like <font color="#ff8000">FF FF FF FF</font> (<font color="#0080ff">255.255.255.255</font>).

It’s working for all known version of mstscax.dll (from 5.1 to Windows 7; version 5.0, which is shipped with Windows 2000 server, doesn’t have this restriction at all).

2\) When FUS (Fast User Switching) is active, you’re getting some ‘strange’ results when connecting via RDP: when you press CAD (Ctrl+Alt+Del), Task Manager will popup; if you try to lock your workstation, for example, by calling <font color="blue">LockWorkStation</font> API, it will just disconnect your session. This is default behavior of Windows XP; let’s see what we can do.

Msgina.dll has an undocumented function, exported by number 3, named ShellIsFriendlyUIActive.  
“&gt;  
It’s a wrapper for function”&gt;which is used to determine if we’re displaying welcome screen or not. In turn, function calls “&gt; then “&gt; What if we’ll overwrite one of them to return true if we’re in RDP session and are active, and false overwise? Let’s write this function:  
“&gt;  
If we compile this function to x64 code, we’ll get the assembler code like below:

<span style="white-space: pre; color: blue; background: white"><span style="color: black">.text:0000000140001170  
.text:0000000140001170</span> <span style="color: gray">; =============== S U B R O U T I N E =======================================</span>  
<span style="color: black">.text:0000000140001170  
.text:0000000140001170  
.text:0000000140001170</span> ; bool IsRpdSessionActive(void)  
<span style="color: black">.text:0000000140001170</span> ?IsRpdSessionActive@@YA\_NXZ proc near <span style="color: #8080ff">; DATA XREF: main+131o</span>  
<span style="color: black">.text:0000000140001170</span> <span style="color: #8080ff">; .pdata:000000014000400Co</span>  
<span style="color: black">.text:0000000140001170  
.text:0000000140001170</span> <span style="color: green">var\_18</span> <span style="color: navy">= dword ptr</span> <span style="color: #008040">-18h</span>  
<span style="color: black">.text:0000000140001170</span> <span style="color: green">var\_10</span> <span style="color: navy">= qword ptr</span> <span style="color: #008040">-10h</span>  
<span style="color: black">.text:0000000140001170</span> <span style="color: green">arg\_0</span> <span style="color: navy">= dword ptr</span> <span style="color: #008040">8</span>  
<span style="color: black">.text:0000000140001170</span> <span style="color: green">arg\_8</span> <span style="color: navy">= byte ptr</span> <span style="color: #008040">10h</span>  
<span style="color: black">.text:0000000140001170  
.text:0000000140001170</span> 48 83 EC 38 <span style="color: navy">sub rsp,</span> <span style="color: green">38h</span>  
<span style="color: black">.text:0000000140001174</span> B9 00 10 00 00 <span style="color: navy">mov ecx,</span> <span style="color: green">1000h</span> ; nIndex  
<span style="color: black">.text:0000000140001179</span> FF 15 D1 0F 00 00 <span style="color: navy">call cs:</span><span style="color: #ff00ff">\_\_imp\_GetSystemMetrics</span>  
<span style="color: black">.text:000000014000117F</span> 85 C0 <span style="color: navy">test eax, eax</span>  
<span style="color: black">.text:0000000140001181</span> 74 3A <span style="color: navy">jz short loc\_1400011BD</span>  
<span style="color: black">.text:0000000140001183</span> 48 8D 44 24 48 <span style="color: navy">lea rax, \[rsp+</span><span style="color: green">38h</span><span style="color: navy">+</span><span style="color: green">arg\_8</span><span style="color: navy">\]</span>  
<span style="color: black">.text:0000000140001188</span> 4C 8D 4C 24 40 <span style="color: navy">lea r9, \[rsp+</span><span style="color: green">38h</span><span style="color: navy">+</span><span style="color: green">arg\_0</span><span style="color: navy">\]</span>  
<span style="color: black">.text:000000014000118D</span> 41 B8 25 00 00 00 <span style="color: navy">mov r8d,</span> <span style="color: green">25h</span>  
<span style="color: black">.text:0000000140001193</span> 48 89 44 24 28 <span style="color: navy">mov \[rsp+</span><span style="color: green">38h</span><span style="color: navy">+</span><span style="color: green">var\_10</span><span style="color: navy">\], rax</span>  
<span style="color: black">.text:0000000140001198</span> 83 CA FF <span style="color: navy">or edx,</span> <span style="color: green">0FFFFFFFFh</span>  
<span style="color: black">.text:000000014000119B</span> 33 C9 <span style="color: navy">xor ecx, ecx</span>  
<span style="color: black">.text:000000014000119D</span> C7 44 24 20 04 00 00 00 <span style="color: navy">mov \[rsp+</span><span style="color: green">38h</span><span style="color: navy">+</span><span style="color: green">var\_18</span><span style="color: navy">\],</span> <span style="color: green">4</span>  
<span style="color: black">.text:00000001400011A5</span> FF 15 B5 0F 00 00 <span style="color: navy">call cs:</span><span style="color: #ff00ff">\_\_imp\_WinStationQueryInformationW</span>  
<span style="color: black">.text:00000001400011AB</span> 84 C0 <span style="color: navy">test al, al</span>  
<span style="color: black">.text:00000001400011AD</span> 74 0E <span style="color: navy">jz short loc\_1400011BD</span>  
<span style="color: black">.text:00000001400011AF</span> 83 7C 24 40 00 <span style="color: navy">cmp \[rsp+</span><span style="color: green">38h</span><span style="color: navy">+</span><span style="color: green">arg\_0</span><span style="color: navy">\],</span> <span style="color: green">0</span>  
<span style="color: black">.text:00000001400011B4</span> 75 07 <span style="color: navy">jnz short loc\_1400011BD</span>  
<span style="color: black">.text:00000001400011B6</span> B0 01 <span style="color: navy">mov al,</span> <span style="color: green">1</span>  
<span style="color: black">.text:00000001400011B8</span> 48 83 C4 38 <span style="color: navy">add rsp,</span> <span style="color: green">38h</span>  
<span style="color: black">.text:00000001400011BC</span> C3 <span style="color: navy">retn</span>  
<span style="color: black">.text:00000001400011BD</span> <span style="color: gray">; —————————————————————————</span>  
<span style="color: black">.text:00000001400011BD  
.text:00000001400011BD</span> <span style="color: navy">loc\_1400011BD:</span> <span style="color: green">; CODE XREF: IsRpdSessionActive(void)+11j</span>  
<span style="color: black">.text:00000001400011BD</span> <span style="color: green">; IsRpdSessionActive(void)+3Dj …</span>  
<span style="color: black">.text:00000001400011BD</span> 32 C0 <span style="color: navy">xor al, al</span>  
<span style="color: black">.text:00000001400011BF</span> 48 83 C4 38 <span style="color: navy">add rsp,</span> <span style="color: green">38h</span>  
<span style="color: black">.text:00000001400011C3</span> C3 <span style="color: navy">retn</span>  
<span style="color: black">.text:00000001400011C3</span> ?IsRpdSessionActive@@YA\_NXZ endp  
<span style="color: black">.text:00000001400011C3 .text:00000001400011C3</span> <span style="color: gray">; —————————————————————————</span></span>

So we can replace the function, for example <font color="blue">CSystemSettings::IsNetwareActive</font> with our one (if we’re not using Netware, which is a pretty rare case:-) ). Since our code is address independent (we can safely place it in any place), we only need to correct the call addresses of <font color="blue">GetSystemMetrics</font> and <font color="blue">WinStationQueryInformationW</font>, because they are located at other addresses in msgina.dll

Now, when we press CAD in RDP session, we’ll see the screen like this:  
[![XPx64RdpLockScreen](http://192.168.40.25:8081/wp-content/uploads/2009/01/xpx64rdplockscreen-1-small.gif)](http://192.168.40.25:8081/wp-content/uploads/2009/01/xpx64rdplockscreen-1.gif)  
while in console sessions FUS screen will be working and looking as usual.

[ Windows XP x64 SP2 Msgina FUS RDP Patch (2328 downloads ) ](http://192.168.40.25:8081/download/windows-xp-x64-sp2-msgina-fus-rdp-patch/?tmstv=1726048918 "Version 1.0")

*Please note that this patch is for msgina.dll from Windows XP x64 SP2 build **5.2.3790.3959** EXACTLY. If you have other version of msgina.dll, you’ll have to repeat all of these steps and create the patch yourself!!!*

Of course, to apply the patches you need to follow the same patching procedure which has described in [part 1](/blog/2008/12/19/windows-xp-x64-terminal-server-patch-part-1-mandatory/)