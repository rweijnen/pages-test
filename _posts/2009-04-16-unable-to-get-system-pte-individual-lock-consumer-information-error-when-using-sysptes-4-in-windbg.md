---
id: 341
title: 'Unable to get System PTE individual lock consumer information error when using !sysptes 4 in WinDbg'
date: '2009-04-16T16:07:07+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2009/04/16/unable-to-get-system-pte-individual-lock-consumer-information-error-when-using-sysptes-4-in-windbg/'
permalink: /2009/04/16/unable-to-get-system-pte-individual-lock-consumer-information-error-when-using-sysptes-4-in-windbg/
views:
    - '6640'
short-url:
    - 'http://bit.ly/ezIING'
categories:
    - Citrix
    - General
    - 'Terminal Server'
---

A few days ago I was troubleshooting some strange problems on a Citrix Server. After some investigation (I will write about that later) it was clear to me that there was a shortage of System Page Table Entries (PTE’s). Using perfmon you can see how many free System PTE’s are available:

![perfcounter](http://192.168.40.25:8081/wp-content/uploads/2009/04/perfcounter.png)

Any value below 5000 is not good, values below 2000 are critical. In my case it wasn’t possible to view processes with Task Manager anymore.

Next I used WinDbg and attached to the Kernel (you can do that with File | Kernel Debug | Local | OK) and issued the !vm command:

[![WinDbg](http://192.168.40.25:8081/wp-content/uploads/2009/04/windbg-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/04/windbg.png)

WinDbg shows us a warning that a lot of PTE allocations have failed, we can also see that there’s enough Paged Pool and Non Paged Pool available.

So how do we find the guilty driver (usually it’s a driver)?

First we need to set a key in the registry

> HKLM\\SYSTEM\\CurrentControlSet\\Control\\Session Manager\\Memory Management  
> Modify the following value to either 0 (disabled) or 1 (enabled).  
> Value Name: TrackPtes  
> Value Type: REG\_DWORD  
> Value Data: 1  
> Radix: Hex

Then we need to reboot and attach WinDbg again and issue the command !sysptes 4  
This gives the following error however on Windows 2003: Unable to get System PTE individual lock consumer information. It happens because the public symbol files from Microsoft’s Symbol Server do not include the \_PTE\_TRACKER structure:

> typedef struct \_PTE\_TRACKER {  
> LIST\_ENTRY ListEntry;  
> PMDL Mdl; PFN\_NUMBER Count;  
> PVOID SystemVa;  
> PVOID StartVa;  
> ULONG Offset;  
> ULONG Length;  
> PFN\_NUMBER Page;  
> struct {  
> ULONG IoMapping: 1;  
> ULONG Matched: 1;  
> ULONG CacheAttribute : 2;  
> ULONG Spare : 28;  
> };  
> PVOID CallingAddress;  
> PVOID CallersCaller;  
> } PTE\_TRACKER, \*PPTE\_TRACKER;
> 
> PTE\_TRACKER PteTracker;

If you have Visual Studio installed you can add this structure to the PDB with the following command:

> cl.exe /Zi /Gz /c /Fdntkrpamp /I”.\\headers” /D\_X86\_=1 PteTracker.c

Or you can download the modified pdb: [ modified ntkrpamp.pdb (1293 downloads ) ](http://192.168.40.25:8081/download/modified-ntkrpamp-pdb/?tmstv=1726048918)