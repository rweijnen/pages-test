---
id: 214
title: 'Converting a volume name to a device name'
date: '2008-12-10T15:41:42+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2008/12/10/converting-a-volume-name-to-a-device-name/'
permalink: /2008/12/10/converting-a-volume-name-to-a-device-name/
views:
    - '7186'
short-url:
    - 'http://bit.ly/gs9j2y'
categories:
    - Delphi
    - General
    - Programming
---

Windows has a couple of different formats for volume names but it is unclear how to convert a Volumename (example: \\\\?\\Volume{GUID}\\) to a DeviceName (example: \\Device\\HarddiskVolume1).

I found at that you can use the QueryDosDevice function but you need to remove the preceeding \\\\?\\ and the trailing \\ of the VolumeName:  
“&gt;