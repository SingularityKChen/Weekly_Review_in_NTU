---
layout: post
title: "[Glean] Five Steps to Make an ASIC for Algorithm X"
description: "Five Steps to Make an ASIC for Algorithm X"
categories: [Glean]
tags: [Linzhi, ASIC]
last_updated: 2021-04-19 16:38:00 GMT+8
excerpt: "Five Steps to Make an ASIC for Algorithm X: Math first, Optimization Target, Hardware-Software Boundary, Building Blocks, Physical Implementation"
redirect_from:
  - /2021/04/19/
---

* Kramdown table of contents
{:toc .toc}
# Five Steps to Make an ASIC for Algorithm X[^1]

## Math first

The reason for going to math first is that <u>hardware may use a fundamentally different way to compute an algorithm.</u>

Example task: Search how many data items X exist within a database P and locate them. Software may need a data structure, maybe a hash table, and many memory accesses. Hardware may be able to produce all results in one clock cycle using a CAM (content-addressable memory).

Many things like search and sort can have a different implementation in hardware.

## Optimization Target

We need to clarify the goal of the optimization. <u>Optimise for high throughput? For low latency? Low power? Low energy? Low cost? Fast time-to-market? Security? High reliability?</u>

You cannot get all of them at once, so you have to pick priorities. For example in a Bitcoin miner, 1st is low energy, 2nd high throughput, 3rd low cost.

After this phase you have constraints, such as budget, target performance, maximum power, target delivery time, etc.

## Hardware-Software Boundary

An algorithm may not benefit from a full hardware implementation. We need to decide which part to run in software, and which to run in hardware, after careful algorithm study.

Normally <u>hardware will handle the major data flow for high-performance. Software can handle complex protocols</u>. However if analysis shows the <u>protocol processing to be the performance bottleneck, that part will also move to hardware.</u>

## Building Blocks

There are three major kinds of building blocks (called IP): <u>logic, memory, IO</u>. Each has nearly infinite types of them available.

Based on project constraints, you can easily rule out most of them.

If no good IP can be found for the target application, we may ask a vendor to customise an IP for our application. Or we can design it ourselves from the transistor level. In Bitcoin miners you find a large ratio of fully custom designed circuit blocks such as dynamic flip-flops.

## Physical Implementation

- Foundry
- Process node
- Floor plan
- Simulation
- Clocking
- Power distribution network
- Timing corner
- Signal integrity
- Temperature range
- Process variation
- Aging
- Pinout
- Packaging
- Thermal management
- Mechanical reliability
- ESD protection
- Verification
- Tapeout
- Process Tuning
- Tester
- Binning/Grade
- PCB
- Assembly
- Software integration
- Burn-in
- Shipping

[^1]: Studying the Feasibility of an ASIC, https://medium.com/@Linzhi/studying-the-feasibility-of-an-asic-82634ac77d61