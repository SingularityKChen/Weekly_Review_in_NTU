---
layout: post
title: "[Glean] Unified Power Format"
description: "The Unified Power Format (UPF) is a published IEEE standard. It is intended to ease the job of specifying, simulating and verifying IC designs that have a number of power states and power islands."
categories: [Glean]
tags: [UPF]
last_updated: 2020-06-29 09:22:00 GMT+8
excerpt: "The Unified Power Format (UPF) is a published IEEE standard. It is intended to ease the job of specifying, simulating and verifying IC designs that have a number of power states and power islands."
redirect_from:
 - /2020/06/29/
---

* Kramdown table of contents
{:toc .toc}
# Unified Power Format[^1]

## What is UPF?

The Unified Power Format (UPF) is a published IEEE standard. It is intended to ease the job of specifying, simulating and verifying IC designs that have a number of power states and power islands.

## What does it do?

UPF is designed to **reflect the power intent of a design** at a relatively high level. UPF scripts describe **which power rails** should be routed to individual blocks, when blocks are expected to be powered up or shut down, **how voltage levels should be shifted** as signals cross from one power domain to another and **whether measures should be taken** to retain register and memory-cell contents if the primary power supply to a domain is removed.

## How is it put into action?

The backbone of UPF, as well as the similar Common Power Format (CPF), is the Tool Control Language (Tcl).

power-aware tools read in the description of **which blocks in a design can be powered up and down independently**. The tools can use that information to determine, for example, how a simulation will behave under different conditions.

For example, a testbench written in SystemVerilog may identify to the simulator that a particular block should be powered down to ensure that other blocks do not access it without checking on power status first.

A transistor-level simulation may use the power definitions to see what happens when supply voltages or substrate bias voltages change. Do all the necessary logic paths meet expected timing when the supply voltage to one block is lowered to save power while others are running at their maximum voltage? Similarly, a static analysis tool may check that the correct level shifters are in place to determine **whether blocks in different power domains can communicate.**

[^1]: https://www.techdesignforums.com/practice/guides/unified-power-format-upf/

