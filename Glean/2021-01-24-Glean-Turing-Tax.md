---
layout: post
title: "[Glean] Turning Tax"
description: "Turning Tax is a term taught in the advanced computer architecture by Paul H J Kelly at IC London. It describes the overhead (performance, cost, or energy) of the universality of the universal computing devices. It can be coused by instructions, data routing, register access and configurable ALU, where we can reduce the Turning Tax."
categories: [Glean]
tags: [Turing Tax, Computer Architecture]
last_updated: 2021-01-24 11:47:00 GMT+8
excerpt: "Turning Tax is a term taught in the advanced computer architecture by Paul H J Kelly at IC London. It describes the overhead (performance, cost, or energy) of the universality of the universal computing devices. It can be coused by instructions, data routing, register access and configurable ALU, where we can reduce the Turning Tax."
redirect_from:
  - /2021/01/24/
---

* Kramdown table of contents
{:toc .toc}
# Turning Tax[^1]

This term is taught by Paul H J Kelly at IC London.

> The “Turing Tax” is a term for the overhead (performance, cost, or energy) of the universality of the universal computing devices.

Which I think are CPUs, GPUs, etc. 

> That is, the performance difference between **a special-purpose device** and **a general-purpose one**, 

The former one is also called DSA (domain special architecture).

> The Turing Tax is actually **the overhead bits due to the general-purpose** nature of the processor, in contrast to a special-purpose digital design.

## Instructions

Store instructions 

+ Fetch them
+ Decode them
+ Maintain PC
+ Handle branches
+ Predict branches
+ Handle branch mis-predictions

## Data routing

Forwarding is used to avoid stalls and is switched by multiplexors, which are determined by instruction decode.

In a special-purpose machine, we might not need all forwarding paths, or not need to switch them, or might place the producer and consumer adjacently, so the wires can be shorter.

## Register access

Instructions use registers to pass values from one operation to the next. Each time a register is used, we have to look the value up in the register file.

In a special-purpose machine, we’d use a piece of wire.

## Configurable ALU

The ALU function is controlled by a signal derived from decoding the instruction, and is a multipurpose unit – that can add, subtract, multiply etc.

In a special-purpose design we would only have **the units we need** and we’d have just **the right number of each kind**.

[^1]: the “Turing Tax”, https://www.doc.ic.ac.uk/~phjk/AdvancedCompArchitecture/Lectures/pdfs/Ch01-TuringTaxDiscussionV02.pdf