---
layout: post
title: "[Emulate] Interface and Channel Design"
description: "This post introduces Interface and Channel Design in SystemC"
categories: [Emulate]
tags: [SystemC, Interface, Channel]
last_updated: 2020-06-16 22:45:00 GMT+8
excerpt: "This post introduces Interface and Channel Design in SystemC"
redirect_from:
  - /2020/06/16/
---

* Kramdown table of contents
{:toc .toc}
# Interface and Channel Design

## Interface and Channel

+ Interfaces and channels provide a flexibly way to model communication and system synchronization
  + Interface classes declare the **access methods**
  + Channels implement the access methods declared within interfaces
+ Interfaces separate the communication from the computation
  + Communication refinement is achieved by allowing one channel to easily be swapped with another

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200616230809.png" alt="Interface and Channel" style="zoom: 67%;" />

## Interface Design

+ Good interface design shall reduce the modeling effort, make **design refinement** easier, and increase **design reuse**
+ Guidelines for designing interface classes
  + Minimize the number of distinctive interfaces
    + Allow the channel implementation be easily swapped 
    + Leave the port instances untouched during the replacement of channels
  + Layer specialized interfaces on more general interfaces 
    + Specialized interfaces inherit from generalized interfaces
  + Use class inheritance to group common interface
    + Create a common base class for different interface classes
  + Use multiple inheritance to create a unified interface class 
    + Define the unified interface to inherit from separate interfaces without adding any new members

### Layered Interface Classes

Allow channel implement interfaces of different specialization.

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200616232652.png" alt="Channel Reuse" style="zoom:67%;" />

### Channel Reuse

Modules use the least specialized interface, but channels that implement more-specialized interfaces can still be attached.

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200616232739.png" alt="Channel Reuse" style="zoom:67%;" />

## Channel Design

+ Primitive channels
  + Derived from `sc_prim_channel`
  + Contain no hierarchy and processes
  + May implement **the request-update mechanism**
  + Faster simulation speed
+ Hierarchical channels
  + Derived from `sc_channel`
  + May **contain hierarchy and processes**
  + Slower simulation speed due to more processes and context switches
+ What if your channel needs request-update mechanism and contains processes, ports, and modules?
  + Split the channel into a primitive channel and a hierarchical channel
  + Instance the primitive channel in the hierarchical one

# Primitive Channels



# Hierarchical Channels

