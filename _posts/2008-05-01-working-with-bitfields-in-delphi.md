---
id: 94
title: 'Working with bitfields in Delphi'
date: '2008-05-01T23:06:00+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2008/05/01/working-with-bitfields-in-delphi/'
permalink: /2008/05/01/working-with-bitfields-in-delphi/
views:
    - '4693'
short-url:
    - 'http://bit.ly/h9MIQH'
categories:
    - Delphi
    - Programming
---

Recently I needed to convert a C header file to Delphi which contained bitfields. Let’s take a look at a sample structure that contains bitfields:  
“&gt;  
It means that there is a DWORD (Cardinal) dwValue1 followed by a bitfield with the size of a ULONG (32 bits). In this bitfield 4 values are defined (BitValue1..4) which are used as boolean’s because the value can offcourse be 0 or 1. Since Delphi doesn’t know a bitfield type the question is how to translate it. Usually it would mean that we simply treat the whole bitfield value as a ULONG and extract the required properties by applying a bitmask (shl/shr). Starting from BDS2006 we can define a record with propertes and use getters and setters. Using this technique we can present boolean values to the user:  
“&gt;  
Code completion shows that the record has one DWORD Value and 4 Boolean Values which is just what we want!  
![CodeCompletion](http://192.168.40.25:8081/wp-content/uploads/2008/05/codecompletion.png)

Offcourse we need to implement the Getters and Setters:  
“&gt;  
We can even add a constructor to it, this can be used to e.g. initialize the record (in the example below we fill with zeroes). Note that only a constructor with at least one argument can be used:  
“&gt;  
So why not use a class instead of record? The answer is that a class is just a pointer we can never pass this to a function, procedure or api call that expects a record. But if we want to support older Delphi versions, like Delphi 6 or Delphi 7 and even Delphi 2005, which are still used a lot we need to find another solution. I came up with (ab)using sets to emulate bitfields, we can do this because a set is actually a set of bits (limited to 256 bits). The example structure could look like this if we use sets:  
“&gt;  
We can use normal set operations to get and set bitvalues:  
“&gt;  
Settings like minimal enum size and record alignment are important because we need to asssure that te record size matches the C structure’s size (especially when using structures with a lot of bitfields. I choose to do this with a litte trick, first I declare some constants:  
“&gt;  
We use these constants to force the correct size, in the example the bitfield was a ULONG which is 32 bits. We add the al32Bit constant to the bitfield:  
“&gt;  
So I thought I had it figured out… until I came to this line in the C header file:  
“&gt;  
So we have a bitfield consisting off multiple bits! This gave me some headaches but I finally came up with the following approach  
“&gt;  
We need a helper function to retreive the numeric value of ColorDepth:  
“&gt;  
The helper function is used like this:  
“&gt;  
Some limitations remain, although I don’t think you are likely to encouter these:

- A Delphi Set can contain at most 256 values.
- The ValueFromBitSet function returns an Int64, so values that do not fit in an Int64 cannot be returned.
- Values in a Set need a unique name.