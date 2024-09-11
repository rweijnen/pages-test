---
id: 216
title: 'Windows XP X64 Terminal Server patch part 1 (mandatory)'
date: '2008-12-19T13:58:17+01:00'
author: daNIL
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2008/12/18/windows-xp-x64-terminal-server-patch-part-1-mandatory/'
permalink: /2008/12/19/windows-xp-x64-terminal-server-patch-part-1-mandatory/
views:
    - '115697'
short-url:
    - 'http://bit.ly/fVogAn'
categories:
    - General
    - 'Terminal Server'
---

Windows XP X64 shares the same binaries with Windows 2003 X64, but Terminal Server has some restrictions on XP. This article shows you how to get rid of them and is based on cw2k ideas from the original Windows XP Terminal Server patch.

<font color="brown">Version 1.1 contains bug#1 fix and is smaller (less bytes are changed).</font>

1\) **Winlogon.exe** contains a function, called <font color="blue">EnumerateMatchingUsers</font> which in turn calls <font color="blue">IsProfessionalTerminalServer</font> function. We need to patch this function to return zero (false):

 .text:0000000100042F77 IsProfessionalTerminalServer proc near <span style="color: green">; CODE XREF: EnumerateMatchingUsers:loc\_10002B44Bp</span><span style="color: black">  
.text:0000000100042F77 </span> <span style="color: #8080ff">; DATA XREF: .pdata:00000001000D01DCo …</span><span style="color: black">  
.text:0000000100042F77</span><span style="color: black">  
.text:0000000100042F77 </span> <span style="color: green">VersionInformation</span><span style="color: navy">= \_OSVERSIONINFOW ptr</span> <span style="color: #008040">-138h</span><span style="color: black">  
.text:0000000100042F77 </span> <span style="color: green">var\_20</span> <span style="color: navy">= word ptr</span> <span style="color: #008040">-20h</span><span style="color: black">  
.text:0000000100042F77 </span> <span style="color: green">var\_ 1E</span> <span style="color: navy">= byte ptr</span> <span style="color: #008040">-1Eh</span><span style="color: black">  
.text:0000000100042F77 </span> <span style="color: green">var\_18</span> <span style="color: navy">= qword ptr</span> <span style="color: #008040">-18h</span><span style="color: black">  
.text:0000000100042F77</span>  
<span style="background: white none repeat scroll 0% 50%; -moz-background-clip: -moz-initial; -moz-background-origin: -moz-initial; -moz-background-inline-policy: -moz-initial; color: blue"><span style="color: black">.text:0000000100042F77</span> 48 81 EC 58 01 00 00 <span style="color: navy">sub rsp</span><span style="color: navy">,</span> <span style="color: green">158h <font color="red">=&gt; 31 C0 C3 xor eax, eax; retn</font></span>  
<span style="color: black">.text:0000000100042F7E</span> 48 8B 05 F3 3A 08 00 <span style="color: navy">mov rax</span><span style="color: navy">, cs:</span>\_\_security\_cookie  
<span style="color: black">.text:0000000100042F85</span> 48 89 84 24 40 01 00 00 <span style="color: navy">mov \[rsp+</span><span style="color: green">158h</span><span style="color: navy">+</span><span style="color: green">var\_18</span><span style="color: navy">\]</span><span style="color: navy">, rax</span>  
<span style="color: black">.text:0000000100042F8D</span> 48 8D 4C 24 20 <span style="color: navy">lea rcx</span><span style="color: navy">, \[rsp+</span><span style="color: green">158h</span><span style="color: navy">+</span><span style="color: green">VersionInformation</span><span style="color: navy">\]</span> ; void \*  
<span style="color: black">.text:0000000100042F92</span> 33 D2 <span style="color: navy">xor edx</span><span style="color: navy">, edx</span> ; int</span>

But it’s not the only place; there is a function named <font color="blue">IsPerOrProTerminalServer</font>. It has a few usages; however we will patch only <font color="brown">1 usage <strike>usages</strike></font> in a function called <font color="blue">MultiUserLogonAttempt</font>:

<span style="background: white none repeat scroll 0% 50%; -moz-background-clip: -moz-initial; -moz-background-origin: -moz-initial; -moz-background-inline-policy: -moz-initial; color: blue"><span style="color: black">.text:0000000100044A91</span> E8 71 E5 FF FF <span style="color: navy">call</span> IsPerOrProTerminalServer  
<span style="color: black">.text:0000000100044A96</span> 85 C0 <span style="color: navy">test eax</span><span style="color: navy">, eax</span>  
<span style="color: black">.text:0000000100044A98</span> 0F 84 C9 00 00 00 <span style="color: navy">jz loc\_100044B67 <font color="brown"><strike>=&gt; 0F 8D C9 00 00 00 jge loc\_100044B67</strike> Not required in version 1.1, causes bug #1 to happen.</font></span>  
…  
<span style="color: black">.text:0000000100044B13</span> E8 EF E4 FF FF <span style="color: navy">call </span> IsPerOrProTerminalServer  
<span style="color: black">.text:0000000100044B18</span> 85 C0 <span style="color: navy">test eax</span><span style="color: navy">, eax</span>  
<span style="color: black">.text:0000000100044B1A</span> 74 0C <span style="color: navy">jz short loc\_100044B28 <font color="red">=&gt; EB 0C jmp short loc\_100044B28</font></span></span>

2\) **Termsrv.dll**. Let’s make Terminal Server think it’s running on a server OS:  
In <font color="blue">ServiceMain</font>, termsrv.dll initializes a global variable called **gbServer**. We need its value to be true (1):

 <span style="background: white none repeat scroll 0% 50%; -moz-background-clip: -moz-initial; -moz-background-origin: -moz-initial; -moz-background-inline-policy: -moz-initial; color: blue"><span style="color: black">.text:000007FF7B877DAF</span> 33 FF <span style="color: navy">xor edi</span><span style="color: navy">, edi</span>  
…  
<span style="color: black">.text:000007FF7B877F77</span> 33 C9 <span style="color: navy">xor ecx</span><span style="color: navy">, ecx</span> ; ConditionMask  
<span style="color: black">.text:000007FF7B877F79</span> C7 05 9D 2C 05 00 1C 01 00 00 <span style="color: navy">mov cs:</span>gOsVersion.dwOSVersionInfoSize<span style="color: navy">,</span> <span style="color: green">11Ch</span>  
<span style="color: black">.text:000007FF7B877F83</span> C6 05 B0 2D 05 00 01 <span style="color: navy">mov cs:</span>gOsVersion.wProductType<span style="color: navy">,</span> <span style="color: green">1</span>  
<span style="color: black">.text:000007FF7B877F8A</span> FF 15 F8 9A FF FF <span style="color: navy">call cs:</span><span style="color: #ff00ff">\_\_imp\_VerSetConditionMask</span>  
<span style="color: black">.text:000007FF7B877F90</span> 48 8D 0D 89 2C 05 00 <span style="color: navy">lea **rcx**</span><span style="color: navy">,</span> gOsVersion ; lpVersionInformation  
<span style="color: black">.text:000007FF7B877F97</span> BA 80 00 00 00 <span style="color: navy">mov edx</span><span style="color: navy">,</span> <span style="color: green">80h</span> ; dwTypeMask  
<span style="color: black">.text:000007FF7B877F9C</span> 4C 8B C0 <span style="color: navy">mov r8</span><span style="color: navy">, rax</span> ; dwlConditionMask  
<span style="color: black">.text:000007FF7B877F9F</span> FF 15 73 95 FF FF <span style="color: navy">call cs:</span><span style="color: #ff00ff">\_\_imp\_VerifyVersionInfoW</span>  
<span style="color: black">.text:000007FF7B877FA5</span> 8B CF <span style="color: navy">mov ecx</span><span style="color: navy">, edi <font color="brown"><strike>=&gt; 31 C9&lt; xor ecx, ecx</strike> Not required in version 1.1 since edi contains zero already</font></span>  
<span style="color: black">.text:000007FF7B877FA7</span> 48 8D 15 B2 AE FF FF <span style="color: navy">lea rdx</span><span style="color: navy">, SubKey</span> <span style="color: gray">; “System\\\\CurrentControlSet\\\\Control\\\\Termin”…</span>  
<span style="color: black">.text:000007FF7B877FAE</span> 85 C0 <span style="color: navy">test eax</span><span style="color: navy">, eax</span>  
<span style="color: black">.text:000007FF7B877FB0</span> 48 8D 44 24 60 <span style="color: navy">lea rax</span><span style="color: navy">, \[rsp+</span><span style="color: green">78h</span><span style="color: navy">+</span><span style="color: green">hKey</span><span style="color: navy">\]</span>  
<span style="color: black">.text:000007FF7B877FB5</span> 41 B9 19 00 02 00 <span style="color: navy">mov r9d</span><span style="color: navy">,</span> <span style="color: green">20019h</span> ; samDesired  
<span style="color: black">.text:000007FF7B877FBB</span> 0F 94 C1 <span style="color: navy">setz cl <font color="red">=&gt; FF C1 90 inc ecx; nop</font></span>  
<span style="color: black">.text:000007FF7B877FBE</span> 45 33 C0 <span style="color: navy">xor r8d</span><span style="color: navy">, r8d</span> ; ulOptions  
<span style="color: black">.text:000007FF7B877FC1</span> 48 89 44 24 20 <span style="color: navy">mov \[rsp+</span><span style="color: green">78h</span><span style="color: navy">+</span><span style="color: green">var\_58</span><span style="color: navy">\]</span><span style="color: navy">, rax</span>  
<span style="color: black">.text:000007FF7B877FC6</span> 89 0D 30 B0 04 00 <span style="color: navy">mov cs:</span>***gbServer*<span style="color: navy">, ecx</span>**</span>

So we have promoted ourselves to server. However on server OS it’s not allowed to disconnect the console (**STATUS\_CTX\_CONSOLE\_DISCONNECT** = **$C00A0027**). So we need to patch 2 places where this code is used:

<span style="background: white none repeat scroll 0% 50%; -moz-background-clip: -moz-initial; -moz-background-origin: -moz-initial; -moz-background-inline-policy: -moz-initial; color: blue"><span style="color: black">.text:000007FF7B889D99</span> 85 C0 <span style="color: navy">test eax</span><span style="color: navy">, eax</span>  
<span style="color: black">.text:000007FF7B889D9B</span> 75 21 <span style="color: navy">jnz short loc\_7FF7B889DBE <font color="red">=&gt; EB 21 jmp short loc\_7FF7B889DBE</font></span>  
<span style="color: black">.text:000007FF7B889D9D</span> 48 8D 4B 18 <span style="color: navy">lea rcx</span><span style="color: navy">, \[rbx+</span><span style="color: green">18h</span><span style="color: navy">\]</span>  
<span style="color: black">.text:000007FF7B889DA1</span> BF 27 00 0A C0 <span style="color: navy">mov edi</span><span style="color: navy">,</span> <span style="color: green">0C00A0027h</span></span>

and one more

<span style="background: white none repeat scroll 0% 50%; -moz-background-clip: -moz-initial; -moz-background-origin: -moz-initial; -moz-background-inline-policy: -moz-initial; color: blue"><span style="color: black">.text:000007FF7B88AA1B</span> 45 85 E4 <span style="color: navy">test r12d</span><span style="color: navy">, r12d</span>  
<span style="color: black">.text:000007FF7B88AA1E</span> 74 0A <span style="color: navy">jz short loc\_7FF7B88AA2A <font color="red">=&gt; EB 0A jmp short loc\_7FF7B88AA2A</font></span>  
<span style="color: black">.text:000007FF7B88AA20</span> BB 27 00 0A C0 <span style="color: navy">mov ebx</span><span style="color: navy">,</span> <span style="color: green">0C00A0027h</span></span>

That’s all for the mandatory patches! To apply them, you need to

- At first, make sure you’re doing it from 64 bit process (for example, 64 bit explorer.exe)! 32 bit processes are redirected to %windir%\\SysWOW64 directory (read <http://msdn.microsoft.com/en-us/library/aa384187(VS.85).aspx>).
- Delete (or move) all termsrv.dll and winlogon.exe files (they are usually in %windir%\\system32\\DllCache, in %windir%\\ServicePackFiles, or maybe somewhere else), except files in %windir%\\system32 . You also need to remove the Windows XP x64 distributive from your CD and/or network location.
- Rename %windir%\\system32 winlogon.exe and termsrv.dll to (for example) winlogon.bak and termsrv.bak. If you’ve done it correctly, you will notice messages like[![wpa](http://192.168.40.25:8081/wp-content/uploads/2008/12/wpa-small.gif)](http://192.168.40.25:8081/wp-content/uploads/2008/12/wpa.gif).
- Copy winlogon.bak and termsrv.bak to some other directory (e.g. c:\\temp) and rename them back to winlogon.exe and termsrv.dll. Apply patch to them.
- Move patched files back to their original location (%windir%\\system32). You will see the WPA message (like picture above) again.
- Reboot
- In case the system will not boot, you’ll need to restore your backups (winlogon.bak and termsrv.bak)

Downloads:<font color="brown"></font>

|  | version 1.1 |
|---|---|
| <strike>[ Windows Xp X64 Terminal Server Mandatory Patch (3512 downloads ) ](http://192.168.40.25:8081/download/windows-xp-x64-terminal-server-mandatory-patch/?tmstv=1726048918 "Version 1.1")</strike> | <strike>version 1.0</strike> |

Registry changes: make sure that the following keys are set:

- *HKLM\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion\\Winlogon\\AllowMultipleTSSessions* =&gt; 1
- *HKLM\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion\\Winlogon\\WinStationsDisabled* =&gt; 0
- *HKLM\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\Console\\fEnableWinStation* =&gt; 1
- *HKLM\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\RDP-Tcp\\fEnableWinStation* =&gt; 1

Some issues remains though: you cannot start a session to localhost (a workaround is connecting to 127.0.0.2), and if you lock your workstation from an RPD Session (not console), you’ll just get disconnected (in case of FUS). I will address these issues in the [part 2](/blog/2009/01/16/windows-xp-x64-terminal-server-patch-part-2-optional/).

<font color="brown"><strike>**Bug #1** Other known issue: a zero-session winlogon will NOT connect to the existing session, but will create a new one instead. This is being investigated currently.</strike> Fixed in version 1.1</font>