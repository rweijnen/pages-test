---
id: 112
title: 'Patch Windows 2003 Terminal Server to allow more than 2 concurrent sessions'
date: '2008-08-31T07:19:48+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2008/08/31/patch-windows-2003-terminal-server-to-allow-more-than-2-concurrent-sessions/'
permalink: /2008/08/31/patch-windows-2003-terminal-server-to-allow-more-than-2-concurrent-sessions/
views:
    - '199816'
short-url:
    - 'http://bit.ly/fo74ja'
categories:
    - 'Terminal Server'
tags:
    - LinkedIn
---

As you might know Windows 2003 Server accepts at most 2 concurrent Terminal Server sessions (and 1 console session) in Remote Administration mode (which is the default). Of course if you switch to Application Mode you can have an unlimited number of sessions but this requires licenses and a license server.

When Terminal Server creates a new session it checks if the new session is either a console session or a help assistant session and if not it allocates a license. The function that performs this check is called CRAPolicy::Logon

<span style="background: white; color: blue"><span style="color: black">.text:7656B494</span>  
<span style="color: black">.text:7656B494</span> <span style="color: gray">; =============== S U B R O U T I N E =======================================</span>  
<span style="color: black">.text:7656B494</span>  
<span style="color: black">.text:7656B494</span> <span style="color: gray">; Attributes: bp-based frame</span>  
<span style="color: black">.text:7656B494</span>  
<span style="color: black">.text:7656B494</span> ; public: virtual long \_\_thiscall CRAPolicy::Logon(class CSession &amp;)  
<span style="color: black">.text:7656B494</span> ?Logon@CRAPolicy@@UAEJAAVCSession@@@Z proc near <span style="color: #8080ff">; DATA XREF: .text:76545B04o</span>  
<span style="color: black">.text:7656B494</span>  
<span style="color: black">.text:7656B494</span> <span style="color: green">arg\_0</span> <span style="color: navy">= dword ptr</span> <span style="color: #008040">8</span>  
<span style="color: black">.text:7656B494</span>  
<span style="color: black">.text:7656B494</span> <span style="color: navy">mov edi</span><span style="color: navy">, edi</span>  
<span style="color: black">.text:7656B496</span> <span style="color: navy">push ebp</span>  
<span style="color: black">.text:7656B497</span> <span style="color: navy">mov ebp</span><span style="color: navy">, esp</span>  
<span style="color: black">.text:7656B499</span> <span style="color: navy">push esi</span>  
<span style="color: black">.text:7656B49A</span> <span style="color: navy">mov esi</span><span style="color: navy">, \[ebp+</span><span style="color: green">arg\_0</span><span style="color: navy">\]</span>  
<span style="color: black">.text:7656B49D</span> <span style="color: navy">push edi</span>  
<span style="color: black">.text:7656B49E</span> <span style="color: navy">mov edi</span><span style="color: navy">, ecx</span>  
<span style="color: black">.text:7656B4A0</span> <span style="color: navy">mov ecx</span><span style="color: navy">, esi</span>  
<span style="color: black">.text:7656B4A2</span> <span style="color: navy">call</span> ?IsSessionZero@CSession@@QBEEXZ <span style="color: gray">; CSession::IsSessionZero(void)</span>  
<span style="color: black">.text:7656B4A7</span> <span style="color: navy">test al</span><span style="color: navy">, al</span>  
<span style="color: black">.text:7656B4A9</span> <span style="color: navy">jnz short loc\_7656B4C2</span>  
<span style="color: black">.text:7656B4AB</span> <span style="color: navy">push</span> <span style="color: green">0</span>  
<span style="color: black">.text:7656B4AD</span> <span style="color: navy">push dword ptr \[esi\]</span>  
<span style="color: black">.text:7656B4AF</span> <span style="color: navy">call</span> \_TSIsSessionHelpSession@8 <span style="color: gray">; TSIsSessionHelpSession(x,x)</span>  
<span style="color: black">.text:7656B4B4</span> <span style="color: navy">test eax</span><span style="color: navy">, eax</span>  
<span style="color: black">.text:7656B4B6</span> <span style="color: navy">jnz short loc\_7656B4C2</span>  
<span style="color: black">.text:7656B4B8</span> <span style="color: navy">push esi</span>  
<span style="color: black">.text:7656B4B9</span> <span style="color: navy">mov ecx</span><span style="color: navy">, edi</span>  
<span style="color: black">.text:7656B4BB</span> <span style="color: navy">call</span> ?UseLicense@CRAPolicy@@AAEJAAVCSession@@@Z <span style="color: gray">; CRAPolicy::UseLicense(CSession &amp;)</span>  
<span style="color: black">.text:7656B4C0</span> <span style="color: navy">jmp short loc\_7656B4C4</span>  
<span style="color: black">.text:7656B4C2</span> <span style="color: gray">; —————————————————————————</span>  
<span style="color: black">.text:7656B4C2</span>  
<span style="color: black">.text:7656B4C2</span> <span style="color: navy">loc\_7656B4C2:</span> <span style="color: green">; CODE XREF: CRAPolicy::Logon(CSession &amp;)+15j</span>  
<span style="color: black">.text:7656B4C2</span> <span style="color: green">; CRAPolicy::Logon(CSession &amp;)+22j</span>  
<span style="color: black">.text:7656B4C2</span> <span style="color: navy">xor eax</span><span style="color: navy">, eax</span>  
<span style="color: black">.text:7656B4C4</span>  
<span style="color: black">.text:7656B4C4</span> <span style="color: navy">loc\_7656B4C4:</span> <span style="color: green">; CODE XREF: CRAPolicy::Logon(CSession &amp;)+2Cj</span>  
<span style="color: black">.text:7656B4C4</span> <span style="color: navy">pop edi</span>  
<span style="color: black">.text:7656B4C5</span> <span style="color: navy">pop esi</span>  
<span style="color: black">.text:7656B4C6</span> <span style="color: navy">pop ebp</span>  
<span style="color: black">.text:7656B4C7</span> <span style="color: navy">retn</span> <span style="color: green">4</span>  
<span style="color: black">.text:7656B4C7</span> ?Logon@CRAPolicy@@UAEJAAVCSession@@@Z endp</span>

So if we want to bypass this license allocation we simple change it to:

<span style="color: black">.text:7656B494</span> <font color="#0000ff">; public: virtual long \_\_thiscall CRAPolicy::Logon(class CSession &amp;)  
</font><span style="color: black">.text:7656B494</span> <font color="#0000ff">?Logon@CRAPolicy@@UAEJAAVCSession@@@Z proc near</font> <span style="color: #8080ff">; DATA XREF: .text:76545B04o</span>  
<span style="color: black">.text:7656B494</span> <span style="color: navy">xor eax</span><span style="color: navy">, eax</span>  
<span style="color: black">.text:7656B496</span> <span style="color: navy">retn</span> <span style="color: green">4</span>  
<span style="color: black">.text:7656B496</span> ?Logon@CRAPolicy@@UAEJAAVCSession@@@Z endp

the binary diff is:

> 0002A894: 8B 31  
> 0002A895: FF C0  
> 0002A896: 55 C2  
> 0002A897: 8B 04  
> 0002A898: EC 00

If you are going to replace termsrv.dll please note that it’s protected by [Windows File Protection](http://support.microsoft.com/kb/222193 "Windows File Protection") so you need to replace it in the following order:

1. Replace termsrv.dll in c:\\windows\\system32\\dllcache
2. If you have the installation cd/dvd (i386) folder copied to your harddrive replace (use the compress command) or remove it there as well
3. Now rename the original file in your system32 folder and place the patched version
4. Reboot

[VPatch](http://www.tibed.net/vpatch/) file: [ Windows Server 2003 VPatch file (50186 downloads ) ](http://192.168.40.25:8081/download/windows-server-2003-vpatch-file/?tmstv=1726048918 "Version 1.0") (of termsrv.dll build 5.2.3790.3959 English language)