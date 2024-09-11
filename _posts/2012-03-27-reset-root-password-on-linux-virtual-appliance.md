---
id: 2567
title: 'Reset Root Password on Linux Virtual Appliance'
date: '2012-03-27T10:52:19+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2012/03/27/reset-root-password-on-linux-virtual-appliance/'
permalink: /2012/03/27/reset-root-password-on-linux-virtual-appliance/
views:
    - '2255'
short-url:
    - 'http://bit.ly/H8qDpB'
categories:
    - General
tags:
    - Linux
    - Suse
    - Ubuntu
---

I needed to login as root on a Linux based virtual appliance to do some troubleshooting. In my case the appliance was running Suse Linux Enterprise.

I booted the VA using the [Ubuntu Live CD](http://www.ubuntu.com/download/ubuntu/download) and opened a Terminal. Then I used the cfdisk tool (sudo cfdisk /dev/sda) to view the partitions:

[![cfdisk /dev/sda](http://192.168.40.25:8081/wp-content/uploads/2012/03/image_thumb23.png "Terminal")](http://192.168.40.25:8081/wp-content/uploads/2012/03/image23.png)

I assumed I needed sda2 so I mounted this partition:

 “&gt;

However if we try to change the password we get an error:

[![cannot open /dev/urandom for reading: No such file or directory | Cannot create salt for blowfish crtpy | Error: Password NOT changed. | passwd: Authentication token manipulation error](http://192.168.40.25:8081/wp-content/uploads/2012/03/image_thumb24.png "chroot /mnt/va")](http://192.168.40.25:8081/wp-content/uploads/2012/03/image24.png)

Apparently the /dev directory is filled while booting to make sure that it’s filled only with available devices.

To be able the change the password I mounted the /dev directory from the Ubuntu Live CD and then it works nicely:

[![mkdir /mnt/va | mount /dev/sda2 /mnt/va | mount --bind /dev /mnt/va/dev | chroot /mnt/va | passwd](http://192.168.40.25:8081/wp-content/uploads/2012/03/image_thumb25.png "Terminal")](http://192.168.40.25:8081/wp-content/uploads/2012/03/image25.png)