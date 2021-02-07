---
layout: post
title: "[Glean] Terms in the Intel Xeon E5-2600 V3 “Haswell-EP” Workstation Die Configurations"
description: "Some terms in Intel Xeon E5-2600 V3 Haswell-EP Workstation Die Configurations, including ACA, LLC, Cbo, SAD, QPI, IIO."
categories: [Glean]
tags: [Haswell, ACA, LLC, Cbo, SAD, QPI, IIO]
last_updated: 2021-02-06 18:33:00 GMT+8
excerpt: "Some terms in Intel Xeon E5-2600 V3 Haswell-EP Workstation Die Configurations, including ACA, LLC, Cbo, SAD, QPI, IIO."
redirect_from:
  - /2021/02/06/
---

* Kramdown table of contents
{:toc .toc}
# Terms in the Intel Xeon E5-2600 V3 “Haswell-EP” Workstation Die Configurations[^2]

![Haswell-EP Die Configurations](https://cdn.wccftech.com/wp-content/uploads/2014/09/Intel-Haswell-EP-Die-Configurations-635x356.png)

From [^1].

More information refer [^2].

## Last Level Cache (LLC)

The processor last level cache comprises a 2.5 MB section for each core slice instantiated but together they represent one logical cache. 

The LLC tracks the MESIF (Modified, Exclusive, Shared, Invalid, and Forwarded) states for <u>maintaining cache coherency between cores and sockets</u>. For any given cache line, the LLC implements core valid bits to track which local core(s) have cached the line in their MLC. Core valid bits are also used by LLC to determine which local core(s) are needed to be snooped during responding to snoop request. The replacement policy is pseudo-least recently used (LRU) with the Invalid way being replaced first. <u>The LLC is a 20 way cache</u> with the ability to <u>allocate any number of ways</u>.

## Caching Agent (Cbo)

The caching agent for the processor socket is address-hashed across Cbo slices. When system BIOS/Firmware disables cores the active Cbo’s are not impacted.

The Cbo provides several functions for agent requests:

+ Request/snoop proxy: Core/PCIe requests are address hashed to select a Cbo to translate and place the request onto the Intel QPI domain. If the last level cache slice attached to that Cbo indicates that a core within the socket owns the line (for a coherent read), the request is snooped to that local core. 
+ Source Address Decoding: The system address decoder is used to determine the destination node id for a given request. The source address decoder is replicated in all Cbo’s.
+ Local Conflict Manager: The Cbo is responsible for ensuring that only one coherent request is issued to the system for a specific cache-line in one socket. This manages conflicts between all the cores local to the socket.

### Source Address Decoder (SAD)

Within the Cbo, requests go through the Source Address Decoder (SAD) at the same time that they are allocating into the TOR and are sent to the LLC. Non-LLC message class types go through the SAD when they are allocating into the TOR as well.

The SAD receives the address, the address space, opcode, and a few other transaction details. It outputs the type of the target of the transaction (DRAM, MMIO, IO, LT, CFG) with proper NodeID or whether it should be serviced in the Local Crab (Ubox).

## Intel® QuickPath Interconnect (Intel® QPI)

The Intel QPI module includes two sub-modules: Intel QPI Agent and the ring stop which is referred to as R3QPI.

The Ring to Intel QPI sub-module (R3QPI) provides several functions:

+ Interface between the Processor Ring and Intel QPI Agent
+ Intel QPI routing
+ Router snoop fanout

## Integrated I/O module (IIO)

The I/O module provides features traditionally supported through chipset components. One of the benefits is that a server does not require auxiliary chipset components aside from the legacy southbridge. The Integrated I/O module provides the following features:

+ **PCI Express Interfaces**: The I/O module incorporates PCI Express interface. The processor can support up to 32 lanes of PCI Express. Following are key attributes of the PCI Express interface:
  + Gen3 speeds at 8 GT/s (no 8b/10b encoding)
  + 2 X16 interfaces, each can be bifurcated down to two x8 or four x4 (or combinations)
+ **DMI2 Interface to the PCH**: The platform requires an interface to the legacy Southbridge (PCH) which provides basic, legacy functions required for the server/workstation platforms and operating systems. Since only one PCH is required for the system, any sockets which do not connect to PCH could use this port as a standard x4 PCI Express 2.0 interface.
+ **Integrated IOAPIC**: Provides support for PCI Express devices implementing legacy interrupt messages without interrupt sharing
+ **Intel® QuickData Technology**: Used for efficient, high bandwidth data movement between two locations in memory or from memory to I/O

[^1]: Intel Xeon E5-2600 V3 “Haswell-EP” Workstation and Server Processors Unleashed For High-Performance Computing, https://wccftech.com/intel-xeon-e52600-v3-haswellep-workstation-server-processors-unleashed-highperformance-computing/
[^2]: Intel® Xeon® Processor E7 v2 2800/4800/8800 Product Family, https://www.intel.com/content/dam/www/public/us/en/documents/datasheets/xeon-e7-v2-datasheet-vol-2.pdf