---
id: 1043
title: 'Unattended Installation of IBM WebSphere MQ'
date: '2010-12-29T16:50:14+01:00'
author: 'Remko Weijnen'
layout: post
guid: 'http://www2.remkoweijnen.nl/blog/?p=1043'
permalink: /2010/12/29/unattended-installation-of-ibm-websphere-mq/
views:
    - '3271'
short-url:
    - 'http://bit.ly/fa2sms'
categories:
    - Packaging
    - 'Unattended Installation'
---

Today I needed to create a silent install for IBM WebSphere MQ, in my case version 6.0.2.10.

I started by reading IBM’s documentation: [WebSphere MQ Unattendend (silent) Installation](http://publib.boulder.ibm.com/infocenter/wmqv6/v6r0/index.jsp?topic=/com.ibm.mq.amqtac.doc/wq10630_.htm "WebSphere MQ Unattended (silent) installation") which desribes that we can [create a response file](http://publib.boulder.ibm.com/infocenter/wmqv6/v6r0/index.jsp?topic=/com.ibm.mq.amqtac.doc/wq10630_.htm "IBM WebSphere MQ") using the SAVEINI parameter.

So I recorded a response file and tested the install using the [USEINI parameter](http://publib.boulder.ibm.com/infocenter/wmqv6/v6r0/index.jsp?topic=/com.ibm.mq.amqtac.doc/wq10630_.htm "WebSphere MQ Using a response file with MsiExec") as indicated by the documentation.

However the installation failed producing only this error message:

[![One or more problems occured. Review the trace and/or log file for details. (AMQ4739)](http://192.168.40.25:8081/wp-content/uploads/2010/12/websphereerror-small.png)](http://192.168.40.25:8081/wp-content/uploads/2010/12/websphereerror.png)

The MSI log file only indicates:

> MSI (s) (F0:64) \[16:29:07:460\]: Product: IBM WebSphere MQ — Installation operation failed.

Which is not very helpfull either but after some searching I found that the installer creates another log file in %temp% called *amqt0001.log* and there I found:

> 16:40:10 MQCA iwiInstallInitSec info: \*\*\*Property REMOVEFEATURES is set to ”; unacceptable for silent installation

So I looked at the response file that was generated and it has this line:

;REMOVEFEATURES=”yes”

I looked up what the documentation says about [REMOVEFEATURES](http://publib.boulder.ibm.com/infocenter/wmqv6/v6r0/index.jsp?topic=/com.ibm.mq.amqtac.doc/wq10630_.htm "WebSphere MQ Response file parameters"):

Must be set to 1 or yes for a silent installation if Internet Gateway, Web Administration Server, or SupportPac MA88 are installed, ***or the installation fails***.

Well that last line is funny but indeed after removing the semicolon (;) the installation was successfull.