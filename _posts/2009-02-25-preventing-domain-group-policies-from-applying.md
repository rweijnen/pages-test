---
id: 325
title: 'Preventing Domain Group Policies from Applying'
date: '2009-02-25T13:31:11+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2009/02/25/preventing-domain-group-policies-from-applying/'
permalink: /2009/02/25/preventing-domain-group-policies-from-applying/
views:
    - '19356'
short-url:
    - 'http://bit.ly/gH3tXk'
categories:
    - General
    - Vista
---

I was just researching a little on how Group Policies are applied in Windows Vista. The client processing is actually done by the Group Policy Client Service. So can a user prevent Domain Policies from being applied by stopping this service?

If you go to the service properties you can see that even a local administrator cannot stop or disable the service:

[![gpsvc](http://192.168.40.25:8081/wp-content/uploads/2009/02/gpsvc-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/gpsvc.png)

The description says: “The service is responsible for applying settings configured by administrators for the computer and users through the Group Policy component. *If the service is stopped or disabled, the settings will not be applied and applications and components will not be manageable through Group Policy*. Any components or applications that depend on the Group Policy component might not be functional if the service is stopped or disabled.”

That sounds good! Let’s try it…

So I used the [RunAsSys](http://blog.delphi-jedi.net/2008/05/08/runassys-10-preview/) tool to run services.msc as system and stopped the service. Apart from a little balloon popup indicating that there was a problem with the service everything seemed to work and Domain Policies were not applied.

Then I tried logging in as a non admin user, and this happened:

[![gpsvcerror](http://192.168.40.25:8081/wp-content/uploads/2009/02/gpsvcerror-small.png)](http://192.168.40.25:8081/wp-content/uploads/2009/02/gpsvcerror.png)

This means that non admin users are not allowed to login when the service isn’t running!

Next I looked in the disassambly of the service (gpsvc.dll) and I noticed this function:

> .text:6F2EA543 ; public: static int \_\_stdcall CSKU::IsDomainIncapableSystem(void)

It calls an exported function of SLC.DLL called [SLGetWindowsInformationDWORD](http://msdn.microsoft.com/en-us/library/aa965835(VS.85).aspx) with parameter pwszValueName of GroupPolicy-License-DomainIncapableSystem. So this function is used to determine if we are running on a system that is not capable (read allowed) to join a domain. This would be the case for the Starter and Home editions of Vista.

So I decided to try patching this value to return always 1, so we change it to:

/Edit: I made an error here: xor eax, eax should be mov eax, 1 (we want the function to return true and not false).

> <span style="color: black">  
> .text:6F2EA543 </span>; public: static int \_\_stdcall CSKU::IsDomainIncapableSystem(void)  
> <span style="color: black">.text:6F2EA543 </span>?IsDomainIncapableSystem@CSKU@@SGHXZ proc near  
> <span style="color: black">.text:6F2EA543 </span><span style="color: green">; CODE XREF: ProcessGPOs(\_GPOINFO \*)+963p  
> </span><span style="color: black">.text:6F2EA543 </span><span style="color: green">; CGroupPolicySession::ApplyGroupPolicyForPrincipal(void \*,void \*,void \*)+124p …  
> </span><span style="color: black">.text:6F2EA543 </span><span style="color: navy">mov eax, 1</span>  
> <span style="color: black">.text:6F2EA545 </span><span style="color: navy">retn </span><span style="color: green">  
> </span><span style="color: black">.text:6F2EA545 </span>?IsDomainIncapableSystem@CSKU@@SGHXZ endp

I tested it and I can login with any account and Domain Policies are not applied!

Here is the [dup2](http://192.168.40.25:8081/2008/12/09/new-universal-patch-method/) file: [ Group Policy Client Service Patch (2512 downloads ) ](http://192.168.40.25:8081/download/group-policy-client-service-patch/?tmstv=1726048918 "Version 1.0")

PS: please check if your license agreement and your country’s law permit it before create and/or applying the patch.

PS2: And don’t tell the Domain Admins 😉

Related article(s): [Registry editing has been disabled by your administrator (not anymore!) ](http://192.168.40.25:8081/2008/08/12/registry-editing-has-been-disabled-by-your-administrator/)