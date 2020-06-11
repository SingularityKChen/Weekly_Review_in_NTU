---
layout: post
title: "[Weekly Review] Different Abstraction Models"
description: "Six different abstraction models by nctu. The models includes specification model, component assembly model, bus arbitration model, cycle accurate computation and RTL model."
categories: [WeeklyReview]
tags: [SystemC, Modelling, TLM]
last_updated: 2020-06-12 00:24:00 GMT+8
excerpt: "Six different abstraction models by nctu. The models includes specification model, component assembly model, bus arbitration model, cycle accurate computation and RTL model."
redirect_from:
  - /2020/06/11/
---

* Kramdown table of contents
{:toc .toc}
# [Different Abstraction Models](http://mapl.nctu.edu.tw/course/ESL/index.php)

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200611195959.png)

## Specification Model – Functional View

+ The system functionality without implementation details
+ Data transfer is modeled by variable accessing without any concept of channel

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200611200114.png" alt="Specification Model" style="zoom: 67%;" />

## Component-Assembly Model – Architectural View

+ Allocation of concurrent processing elements and mapping of processes
+ Data transfer is achieved by message passing channels
+ Message-passing channels
  + An abstract implementation of communication focusing on data transaction
  + No cycle-accurate, pin-accurate details, and no specific bus protocol

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200611200245.png" alt="Component-Assembly Model" style="zoom: 67%;" />

## Bus Arbitration Model – Architectural View

+ Channels between PEs are realized by an abstract bus
  + Require design decision in both computation and communication
+ An abstract bus
  + Data transfers are still implemented by message passing
  + Bus protocol is simplified as blocking and non-blocking I/O
  + Arbiter is required to resolve bus conflicts
  + No cycle-accurate, pin-accurate details

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200611232428.png" alt="Bus Arbitration Model" style="zoom:67%;" />

## Bus Functional Model – Micro-architectural View

+ Abstract bus channel is inline with a cycle-/pin-accurate protocol channel
  + Wires of the bus are instantiated with variables/signals
  + Data transfer follows the time/cycle-accurate sequence
  + Provide interface functions for all abstract bus transactions
  + Wrappers convert data transfer from higher level of abstraction (PEs) to lower level of abstraction (Protocol Channel)

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200611232552.png" alt="Bus Functional Model" style="zoom:67%;" />

## Cycle-Accurate Computation – Micro-architectural View

+ The PEs are cycle- and pin-accurate
  + Dedicated hardware IPs are modeled at register transfer level
  + Programmable processors are modeled by instruction set simulator
  + Wrappers convert data transfer from higher level of abstraction (abstract bus) to lower level of abstraction (PEs)

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200612001814.png" alt="Cycle-Accurate Computation" style="zoom:67%;" />

## RTL Model – Pure Micro-architecture View

Both computation and communication are pin- and cycle-accurate

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200612001906.png" alt="RTL Model" style="zoom:67%;" />

## From Specification to Micro-architecture

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200612002854.png" style="zoom:67%;" />

### Component Assembly

+ Based on the algorithm analyses, we perform the following tasks
+ Partition the algorithm into SW/HW tasks
  + Select general purpose CPU or DSP based on the SW characteristics
  + Choose RTOS if necessary
+ Design IPs or select IPs from library according to the HW tasks
  + Define the functionalities of each IP
  + Define the interfaces and the data to be exchanged between IPs
  + Estimate functional delays in IPs

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200612002941.png" alt="Component Assembly" style="zoom:67%;" />

### Communication Exploration

+ Decision of interconnect structure 
  + Back-door connections or centralized buses
  + Assign bus-accessing properties for each IP (master or slave)
  + Decide the bus arbitration policy
+ Estimate functional delays in communication

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200612003119.png" alt="Communication Exploration" style="zoom:67%;" />

### Protocol Refinement

+ Inline abstract bus with protocol channel
  + Determine the pin- and cycle-accurate bus protocol
  + Work out the details of the bus control signals
+ Wrappers are used to bridge the models of different abstractions
+ Extract delays in communication from micro-architecture

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200612003334.png" alt="Protocol Refinement" style="zoom:67%;" />

### IP Refinement

+ The IPs are refined to pin- and cycle-accuracy
+ Delays in computation are extracted from micro-architecture
+ The embedded SW is optimized to achieve real-time performance

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200612003356.png" alt="IP Refinement" style="zoom:67%;" />

### IP Replacement

+ Some IPs are modeled with pin- or cycle-accuracy
  + The IPs are replaced or refined one by one
+ Wrappers are used to bridge the models of different abstractions

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200612003431.png" alt="IP Replacement" style="zoom:67%;" />

### Communication Refinement

+ Inline abstract bus with protocol channel
  + Determine the pin- and cycle-accurate bus protocol
  + Work out the details of the bus control signals
+ Extract delays in communication from micro-architecture
+ Wrappers are used to bridge the models of different abstractions

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200612003453.png" alt="Communication Refinement" style="zoom:67%;" />