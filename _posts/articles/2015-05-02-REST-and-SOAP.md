---
layout: post
title: "SOAP and REST"
categories: article
excerpt: ""
tags: ["REST", "SOAP", "JSON", "Web Service"]
---

Recently I was asked in a meetup about REST versus SOAP. This I thought hasn't encountered for some time and it's interesting that hasn't entered the radar. Before REST is popular to the extent that it is ubquitious, SOAP seems to be fading into background. 

SOAP = Simle Object Access Protocol
REST = Representation State Transfer

Well, for one SOAP only works on XML data format, which is not very refined with the Mongo document-based type of data storage. REST works well with JSON (not just JSON for the XML situations!) which tends to be loose on.

[3 items](http://spf13.com/post/soap-vs-rest) not supported natively in REST: 

* security (stronger than SSL), 
* transaction (rollback, ACID - atomicity, continuity, isolation, durability), 
* reliable messaging (such as retry and sequencing like TCP)

In addition, one can cache response on get operations REST but not so for SOAP


