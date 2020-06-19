---
layout: post
title: "[Emulate] Refinement of Computation and Communication"
description: "This post introduces Refinement of Computation and Communication in SystemC."
categories: [Emulate]
tags: [SystemC, Refinement, Adapters, Converters]
last_updated: 2020-06-20 00:39:00 GMT+8
excerpt: "This post introduces Refinement of Computation and Communication in SystemC. Including different kinds of communication refinement, such as channel refinement, module refinement, hw-hw refinement, sw-sw refinement, hw-sw refinement."
redirect_from:
  - /2020/06/19/
---

* Kramdown table of contents
{:toc .toc}
# Refinement of Computation and Communication[^1]

## Introduction

### Communication Refinement

+ Expand an abstract communication into an actual implementation
  + Example: transition from `sc_fifo` to `hw_fifo`
  + Implementation can be either software or hardware
    + Hardware: **a shift register** or **a RAM** with some control logic
    + Software: **a certain memory range** with a modulo addressing scheme
+ No simple one-size-fits-all approach
+ Key principles
  + Use well-defined and well-understood interfaces
  + Intelligent use and reuse of adapters and converters

### Adapters – Channel Refinement

+ A **hierarchical channel** that translates the interface it implements into port accesses bound to a different interface
  + Example: the `read()`/`write()` methods in the `hw_fifo_wrapper` can be encapsulated in an adapter, respectively
+ Adapters can be easily reused and a small library of adapters is sufficient
  + Typically only a finite number of different interfaces are used
+ Two things that can be done with an adapter for refinement
  + Introduce an additional level of hierarchy
    + Wrap the refined channel and the adapters connected to it
  + Merge the adapters into the calling modules
    + Move ports, processes, methods, and data fields to the calling processes 
    + Replace calls to the adapter’s methods with the new member functions

### Converters – Module Refinement

+ A **module** that translates a set of ports to another in order to make the refined module and communication channel fit together
  + The refined model could be a piece of an existing IP and have a different set of ports than its abstract counterpart
+ Things that can be done with an converter for refinement
  + Wrap a refined module and a converter as a new module that matches the channel’s footprint
  + Merge an adapter to form a new, refined converter
    + A converter may accesses an adapter via the adapter’s interface
+ Converters versus Adapters
  + Converters tend to be a component in **the final implementation**
  + Adapters are often **temporal objects**

### General Process of Refinement

1. Select and Replace
   + Select a refined implementation for a component
   + Replace the original component with the refined implementation
2. Insert adapters or converters (optional)
   + Make channel and module play together
   + Adapters and converters should be stored in an accessible library
3. Analyze the I/O functionality and performance
   + Verify the functionality and system performance
4. Flatten (optional)
   + Ungroup the hierarchy of the wrapped channel
   + Merge adapters and connected modules
5. Wrap (optional)
   + Wrap the refined channel and the set of connected adapters to create a new channel for software implementation
6. Merge (optional)
  + Refine an abstract communication protocol (transaction-level) towards a hardware implementation (pin-level)
  + Merge adapters into the calling modules (protocol inline)
7. Restructure control flow (optional)
   + High level module is generally described in a sequential manner using blocking read()/write() • first_output->write(some_data) • second_output->write(some_data)
   + Sequential execution might not be suited for hardware
   + One may want to refine control flow to exploit more parallelism
     + first_output->initiate_transmission(some_data)
     + second_output->initiate_transmission(some_data) 
     + wait_for_completion(first_port, second_port)
8. Analyze Implementation
   + Check both functionality and I/O performance
   + Analyze the partial or entire system for its implementation characteristics such as area, latency, clock frequency, and power dissipation

## Hardware-Hardware Communication Refinement (From Abstract Model to RTL)

## Software-Software Communication Refinement (From SystemC to Plain C)

## Hardware-Software Communication Refinement
+ Hardware Part: From Abstract Model to RTL
+ Software Part: From SystemC to Plain C



[^1]: [IOC5080(5940) System Model Design and Verification](http://mapl.nctu.edu.tw/course/ESL/index.php), Department of Computer Science, National Chiao-Tung University