---
id: 730
title: 'Programmatically Create Aligned Partitions'
date: '2010-11-04T18:14:56+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=730'
permalink: /2010/11/04/programmatically-create-aligned-partitions/
views:
    - '3075'
short-url:
    - 'http://bit.ly/gF5f0T'
categories:
    - Altiris
    - VMWare
---

After deploying Virtual Machines from a template and adding disks the next Task was to create and format the partitions.

In a VMWare environment it is very important to assure that the partitions are aligned. VMWare has a nice document that explains the details called [Performance Best Practices for VMware vSphere 4.0](http://www.vmware.com/pdf/Perf_Best_Practices_vSphere4.0.pdf "Performance Best Practices for VMware vSphere 4.0").

Basically the recommendation is to align partition on a 64 KB boundary, not only for VMWare itsself but also for the guests.

Of course we could do this manually but I wanted to run this task of from my Deployment Server to automate the following:

- <div>Create aligned partitions on all extra disks of the maximum size</div>
- <div>Format, assign drive letter and assign a label</div>
- <div>A safeguard to prevent overwriting existing Data</div>

Well enough talk let’s go the scripts! They are written as bat files that can be executed directly in Altiris as an embedded script but of course you don’t need Altiris to use them.  
“&gt;  
And the second script:  
“&gt;