---
layout: post
title: "[Glean] Round-Robin Arbitration"
description: "Round robin arbitration is a scheduling scheme which gives to each requestor its share of using a common resource for a limited time or data elements."
categories: [Glean]
tags: [RoundRobin]
last_updated: 2020-06-29 09:23:00 GMT+8
excerpt: "Round robin arbitration is a scheduling scheme which gives to each requestor its share of using a common resource for a limited time or data elements."
redirect_from:
  - /2020/06/25/
---

* Kramdown table of contents
{:toc .toc}
# Round-Robin Arbitration

Round robin arbitration is a scheduling scheme which gives to each requestor its share of using a common resource for a limited time or data elements.[^1]

![](http://rtlery.com/sites/default/files/queueing_fifos_and_arbiter.png)

The simplest form of round robin arbiter is based on assignment of a fixed time slot per requestor; this can be implemented using a circular counter. 

Alternatively, **the weighted round robin arbiter** (WRR) is defined to allow a specific number X of data elements per each requestor, in which case X data elements from each requestor would be processed before moving to the next one. 

Round robin arbiters are typically used for arbitration for shared resources, load balancing, queuing systems, and resource allocation.

[^1]: RTLery, http://rtlery.com/articles/round-robin-arbitration