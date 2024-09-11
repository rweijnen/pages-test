---
id: 3472
title: 'non-existent schema type &#8216;Snmp!System.SnmpVarBindsSchema&#8217; error in SCOM'
date: '2014-07-13T23:50:59+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=3472'
permalink: /2014/07/13/non-existent-schema-type-snmpsystem-snmpvarbindsschema-error-scom/
short-url:
    - 'http://bit.ly/1nljx5L'
views:
    - '253'
categories:
    - Uncategorized
tags:
    - 'Management Pack'
    - SCOM
    - 'Visual Studio'
    - VSAE
---

![System Center Operations Manager Logo](http:///192.168.40.25:8081/wp-content/uploads/2014/07/image_thumb.png?zoom=1.5&resize=89%2C95 "SCOM Logo")Today I encounterd what seems to be a bug in the [System Center 2012 Visual Studio Authoring Extensions](http://www.microsoft.com/en-us/download/details.aspx?id=30169). I wanted to define a Performance Collection Rule that reads out the percentage of free memory from an SNMP device.

Since the device returns only the percentage of used memory I needed to use the [ComputedPerfProvider](http://msdn.microsoft.com/en-us/library/jj130485.aspx) provider to substract the used memory percentage from 100.

I could of course report used memory instead of free memory but I wanted the resulst to appear in the default SCOM Performance View, which only lists Free Memory:

[![System Center Operations Manager | Default Performance View](http://192.168.40.25:8081/wp-content/uploads/2014/07/image_thumb1.png "SCOM Performance View")](http://192.168.40.25:8081/wp-content/uploads/2014/07/image1.png)

The XML to calculate the free percentage is quite easy and looks like this:

 “&gt;

However when I tried to add this to the Configuration XML in the Data Source Configuration I got the following error message: *The configuration of element ‘System.NetworkManagement.ComputedPerfProvider’ is including a non-existent schema type ‘Snmp!System.SnmpVarBindsSchema’:*

[![The configuration of element 'System.NetworkManagement.ComputedPerfProvider' is including a non-existent schema type 'Snmp!System.SnmpVarBindsSchema'](http://192.168.40.25:8081/wp-content/uploads/2014/07/SNAGHTML13b7aae6_thumb.png "Microsoft Visual Studio")](http://192.168.40.25:8081/wp-content/uploads/2014/07/SNAGHTML13b7aae6.png)

Changing the Referenced Management Packed version to 2012SP1 or even 2012R2 did not fix the issue so I presume this is a bug in the authoring extensions.

As a workaround you can insert the XML directly into the .mptg file (do not edit the .mptg.mpx file as it will be overwritten on next build).

To do this close the project in Visual Studio and delete the .mptg.mpx file. Now take the XML and replace the &lt; and &gt; sign with html entities and remove indenting and line breaks. In my case the result looks like this:

> `&lt;Interval&gt;300&lt;/Interval&gt;&lt;NoOfRetries&gt;3&lt;/NoOfRetries&gt;&lt;Timeout&gt;200&lt;/Timeout&gt;&lt;SnmpVarBinds&gt;&lt;SnmpVarBind&gt;&lt;OID&gt;.1.3.6.1.4.1.38797.100.1.5.0&lt;/OID&gt;&lt;Syntax&gt;0&lt;/Syntax&gt;&lt;Value VariantType="8" /&gt;&lt;/SnmpVarBind&gt;&lt;/SnmpVarBinds&gt;&lt;ComputedPerformanceValue&gt;&lt;Subtraction&gt;&lt;NumericValue&gt;&lt;Value&gt;100&lt;/Value&gt;&lt;/NumericValue&gt;&lt;NumericValue&gt;&lt;XPathQuery Type="Integer"&gt;SnmpVarBinds/SnmpVarBind\[OID=".1.3.6.1.4.1.38797.100.1.5.0"\]/Value&lt;/XPathQuery&gt;&lt;/NumericValue&gt;&lt;/Subtraction&gt;&lt;/ComputedPerformanceValue&gt;&lt;ObjectName&gt;Memory&lt;/ObjectName&gt;&lt;CounterName&gt;Free Memory %&lt;/CounterName&gt;`

Sadly it’s not possible to edit this XML from Visual Studio, even after inserting it this way. So futures updates must follow the same process.

Happy authoring!