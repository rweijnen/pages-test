---
id: 1999
title: 'Cannot achieve Exchange Server authentication'
date: '2011-08-17T16:49:53+02:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/2011/08/17/cannot-achieve-exchange-server-authentication/'
permalink: /2011/08/17/cannot-achieve-exchange-server-authentication/
views:
    - '4859'
short-url:
    - 'http://bit.ly/nBlcat'
categories:
    - Exchange
tags:
    - Cas
    - Cisco
    - Edge
    - Exchange
    - Exchange2010
---

[![Exchange Logo](http://192.168.40.25:8081/wp-content/uploads/2011/06/SNAGHTML1ca684c_thumb.png?9d7bd4 "Exchange Logo")](http://192.168.40.25:8081/wp-content/uploads/2011/06/SNAGHTML1ca684c.png?9d7bd4)

I was testing outgoing mail flow in my new Exchange 2010 setup, which should go from the CAS Servers to the Edge server in the DMZ.

After configuring the Edge subscription I noticed that outgoing mails got stuck in the queue with the following error:

“*451 4.4.0 Primary target IP address responded with: "451 5.7.3 Cannot achieve Exchange Server authentication." Attempted failover to alternate host, but that did not succeed. Either there are no alternate hosts, or delivery failed to all alternate hosts.*“

I verified that name resolution back and forth was ok and that I could communicate on port 25, 50389 and 50636.

Then I tried to telnet from a CAS server to the Edge server on port 25 and I noticed that there was some kind of smtp filtering active.

[![Cisco Logo](http://192.168.40.25:8081/wp-content/uploads/2011/08/image_thumb3.png "Cisco Logo")](http://192.168.40.25:8081/wp-content/uploads/2011/08/image3.png)The most common kind is from Cisco, where it’s called either smtp fixup, (e)smtp inspection or CSC inspection.

You can recognize it with a telnet connection because server name, version etc are masked with asterix character:

[![cisco esmtp inspection](http://192.168.40.25:8081/wp-content/uploads/2011/08/SNAGHTML1fb8ab33_thumb.png "cisco esmtp inspection")](http://192.168.40.25:8081/wp-content/uploads/2011/08/SNAGHTML1fb8ab33.png)

The problem is that esmtp inspection drops packets for TLS encryption (which is used between CAS and Edge).

I checked the Cisco switch and in the config there was an inspect esmtp statement in the global\_policy policy-map.

After modifying the configuration the communication went fine:

 “&gt;

For more details see [PIX/ASA 7.x and above: Mail (SMTP) Server Access on the DMZ Configuration Example](http://www.cisco.com/en/US/products/hw/vpndevc/ps2030/products_configuration_example09186a00806745b8.shtml)