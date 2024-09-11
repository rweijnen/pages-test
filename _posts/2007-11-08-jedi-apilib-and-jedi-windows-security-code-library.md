---
id: 55
title: 'JEDI Apilib and JEDI Windows Security Code Library'
date: '2007-11-08T12:35:32+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2007/11/08/jedi-apilib-and-jedi-windows-security-code-library/'
permalink: /2007/11/08/jedi-apilib-and-jedi-windows-security-code-library/
views:
    - '2340'
short-url:
    - 'http://bit.ly/icUuhX'
categories:
    - Delphi
    - Programming
---

It has been a little silent on the [<font color="#22229c">JEDI Apilib project</font>](http://sourceforge.net/projects/jedi-apilib/) lately, but this will change!

We had some change in the team members: Marcel van Brakel, founder and large contributor of the project has signed off because he no longer actively uses Delphi. Christian Wimmer has joined the team and he is a very promising member.

Christian has been working on a new include model (optional) of the Jedi Apilib which has the advantage that you only need to use one unit (JwaWindows) for the whole library.

Chris has also published the JEDI Windows Security Code Library (Jwscl). This is library that tremendously simplifies using Win32 API calls from Delphi. At this point the library contains:

- Windows Version
- Token
- Impersonation
- Login
- SID
- Access Control List
- Security Descriptor
- Owner, Group, DACL, SACL
- WindowStation
- Desktop
- LSA
- Rights mapping
- Secured Objects Files, Registry (+Inheritance), etc.
- Credentials (Login Dialog)
- Encryption (MS Crypto API)
- Well Known SIDs
- Privileges
- Security Dialogs (The “ACL Editor” you see on the security yab when you rightclick object in Explorer)
- Terminal Sessions
- Unicode + Ansicode
- Vista Elevation
- Vista Integrity Level

Although Jwscl is in beta stage it is already very well useable.

Work is in progess for a new website and blog for the projects. Meanwhile you download from SourceForge (svn). More info on the Security Library can be found here: [<font color="#22229c">http://www.delphipraxis.net/post803238.html</font>](http://www.delphipraxis.net/post803238.html#803238)