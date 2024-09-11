---
id: 1757
title: 'Stuff from an ancient past'
date: '2011-05-18T10:08:35+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/05/18/stuff-from-an-ancien-past/'
permalink: /2011/05/18/stuff-from-an-ancien-past/
views:
    - '1947'
short-url:
    - 'http://bit.ly/kfJeuL'
categories:
    - Programming
---

I just found a very old backup file containing old source code for a few tools I wrote ages ago.

This was in 1997 on my first job for a company called PTT Telecom (the Dutch Telecoms) and I wrote some tools to make life easier.

They were all written in Turbo Pascal and supported Long Filenames when running under Windows ’95 (there was a trick to do that under DOS).

The first tool was called Retreive Tool, it parsed a backup file from a private branch exchange (PBX) and could make reports about Licensing, the hardware in the PBX, Extension numbers and their hardware positions and so on.

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/05/image_thumb13.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/05/image13.png)

The hardware report displayed a “picture” of the chassis with the prints, my dos font doesn’t correctly draw all lines in 2011 though:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/05/image_thumb14.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/05/image14.png)

Another tool I found was to fix a bug for C/DOS or Concurrent Dos, an MS-Dos compatible OS that could run 4 Applications concurrently.

However the O/S didn’t run with certain Toshiba PC’s that had lots of memory (lots was 8 MegaByte in those days) so I wrote a little tool that wrote fake memory values in the CMOS (Bios) so there was less memory visible to the OS. This involved direct writing to the CMOS memory and correcting the checksum afterwards.

Since CMOS memory is not part of the PC’s main memory it could only be accessed by reading and writing to an I/O port.

I am not sure what the purpose of the next tool was but it changed the default connection type for something:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/05/image_thumb15.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/05/image15.png)

Last one I am showing here is a tool called dbRepair and it’s purpose was to repair corrupted database tables (FoxPro) from a call logging system called TABS:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/05/image_thumb16.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/05/image16.png)