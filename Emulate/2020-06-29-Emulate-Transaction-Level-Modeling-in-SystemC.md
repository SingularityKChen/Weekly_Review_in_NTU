---
layout: post
title: "[Emulate] Transaction Level Modeling in SystemC"
description: "This is the 6th SystemC lecture in National Chiao-Tung University."
categories: [Emulate]
tags: [SystemC, TLM]
last_updated: 2020-06-25 22:19:00 GMT+8
excerpt: ""
redirect_from:
  - /2020/06/29/
---

* Kramdown table of contents
{:toc .toc}
# Transaction Level Modeling in SystemC[^1]

+ A high-level approach to model digital systems
  + Care more on **what data are transferred to and from what locations**
  + Care less on the actual protocol used for data transfer
+  Features 
  + Details of communication are separated from details of computation
  + Communication mechanisms are **modeled as channels** 
  + Low-level details of information exchanged **are hidden in channels**
  + Pin level details at the structural boundary **are abstracted into interface**
  + Transaction requests take place **by calling interface functions**
  + Synchronization details of channels are typically abstracted into blocking and/or non-blocking I/O
+ Advantages 
  + Enable **high simulation speed** by hiding uninteresting details

## Timed TLM–The Very Simple Bus

### Introduction of The Very Simple Bus

+ Although this example is very simple and may not be practical, it provides us an essential concept about TLM in SystemC
+ Some behaviors of the real bus such as arbitration, split transactions, and memory wait states are not considered
+ Memory is modeled as a memory array within the bus rather than a memory module external to the bus
+ Contention, arbitration, interrupts, and cycle-accuracy can be modeled with TLM without resorting to pin-accuracy

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200629231609.png)

### Implementation of The Very Simple Bus



### Suppression of Uninteresting Details

+ Burst transfer may take many cycles to complete
+ The bus is merely doing routine works
+ Clients that have pending bus requests are just waiting

## Timed TLM – The Simple Bus Design

### Introduction of the Simple Bus Design

### Structure of the Simple Bus Design

### Features of the Simple Bus Design

### Ideas behind the Simple Bus Model

## Master Interface

### Introduction of Master Interface

### Blocking Master Interface

### Return Values of Master Interface Methods

### Non-Blocking Master Interface

### Direct Master Interface

## Slave and Arbiter Interfaces

## Concepts of Operations

## Implementation of Masters

## Implementation of Bus

## Implementation of Slaves

## High-Performance Modeling Techniques

### Introductions



### Comparisons between TLM and RTL



### Common Questions



[^1]: [IOC5080(5940) System Model Design and Verification](http://mapl.nctu.edu.tw/course/ESL/index.php), Department of Computer Science, National Chiao-Tung University