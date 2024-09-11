---
id: 1530
title: 'AES-NI Benchmarks'
date: '2011-03-11T11:35:20+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/03/11/aes-ni-benchmarks/'
permalink: /2011/03/11/aes-ni-benchmarks/
views:
    - '6156'
short-url:
    - 'http://bit.ly/fh4Qos'
categories:
    - General
---

As you may know, recent Intel processors have an extension to the x86 instruction set called Advanced Encryption Standard Instruction Set ([AES-NI](http://en.wikipedia.org/wiki/AES_instruction_set)).

AES-NI is basically hardware support for AES based encryption and because I had a chance to run some benchmarks on differing systems I was curious what the impact of AES-NI would be.

I used [TrueCrypt](http://www.truecrypt.org/) for running the benchmarks because this is a real life application and it had support for AES-NI.

I first ran the benchmark on a laptop with an Intel Core2 DUO (P9700 2,80 GHz):

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb2.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image2.png)

[![clip_image002[5]](http://192.168.40.25:8081/wp-content/uploads/2011/03/clip_image0025_thumb.jpg "clip_image002[5]")](http://192.168.40.25:8081/wp-content/uploads/2011/03/clip_image0025.jpg)

The next system was an Intel Core i7 Q740 (Quad Core with [Hyperthreading](http://en.wikipedia.org/wiki/Hyper-threading), so 8 in total) machine.

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb3.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image3.png)

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb4.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image4.png)

This is a real fast machine, and the Processor score confirms this:

[![image](http://192.168.40.25:8081/wp-content/uploads/2011/03/image_thumb5.png "image")](http://192.168.40.25:8081/wp-content/uploads/2011/03/image5.png)

So I was expecting a huge performance increase in the benchmarks but it’s only slightly faster:

[![clip_image002[7]](http://192.168.40.25:8081/wp-content/uploads/2011/03/clip_image0027_thumb.jpg "clip_image002[7]")](http://192.168.40.25:8081/wp-content/uploads/2011/03/clip_image0027.jpg)

The reason is that unlike most other i7 CPU’s the Quadcore (mobile) CPU’s do not have AES-NI support.

The big surprise was the benchmark on a machine that was in theory slower but did have AES-NI support. It had an i7-640M CPU (Dual Core with HyperThreading):

[![clip_image002](http://192.168.40.25:8081/wp-content/uploads/2011/03/clip_image002_thumb.jpg "clip_image002")](http://192.168.40.25:8081/wp-content/uploads/2011/03/clip_image002.jpg)

That’s an impressive difference (around 5 times faster on AES)!

So my conclusion is that *if you use Encryption software a lot (eg if you use software based Drive Encryption) an AES-NI enabled CPU is preferred*.

Intel publishes a list of AES-NI enabled CPU’s [here](http://ark.intel.com/search/advanced?AESTech=true) (link updated 03-01-2013).

**Update 03-01-2013**: Intel lists that certain processors can support AES New Instructions with a Processor Configuration update, in particular, i7-2630QM/i7-2635QM, i7-2670QM/i7-2675QM, i5-2430M/i5-2435M, i5-2410M/i5-2415M.

I’ve also ran the Benchmark on my new laptop with an Intel 2960XM which comes to a nice 3,2 GB/s for AES:

[![Intel2960XM](http://192.168.40.25:8081/wp-content/uploads/2011/03/Intel2960XM-300x234.png)](http://192.168.40.25:8081/wp-content/uploads/2011/03/Intel2960XM.png)