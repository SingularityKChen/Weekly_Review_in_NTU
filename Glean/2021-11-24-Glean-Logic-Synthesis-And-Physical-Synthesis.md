---
layout: post
title: "[Glean] Logic Synthesis, Physical Synthesis"
description: "The difference between Logic Synthesis and Physical Synthesis."
categories: [Glean]
tags: [Logic Synthesis, Physical Synthesis, Fusion Compiler]
last_updated: 2021-12-07 10:14:00 GMT+8
excerpt: "The difference between Logic Synthesis and Physical Synthesis."
redirect_from:
  - /2021/11/24/
---

* Kramdown table of contents
{:toc .toc}
# Logic Synthesis, Physical Synthesis

## Quick Overview[^1]

**Logic synthesis** creates **a netlist** of gates from RTL Verilog. It also includes other steps such as technology mapping where the gates are selected from a set of libraries provided and timing/area/power optimisation.

**Physical synthesis** transforms the gate level netlist to **a layout** that can be realised (etched) on silicon. It includes floor planning, placement (fixing location of the gates), routing ( wires in gate level netlist are assigned metal layers etc), clock tree synthesis and multiple different steps of local and global optimisations.

## Definition of Logic Synthesis[^2]

**Logic synthesis** takes as an input <u>a description of a circuit</u> expressed in a high-level language such as Verilog or VHDL. Other inputs include <u>timing constraints</u> for the design as well as the <u>specific target implementation technology</u>. Logic synthesis software then analyzes these inputs and maps them to a particular set of <u>interconnected logic elements</u> taken from cell libraries that are also provided as inputs to the process. 

## Objective of Logic Synthesis[^2]

Objectives of logic synthesis include optimization of <u>timing, area, and power</u> of the resultant design. The output of logic synthesis was then given to a place-and-route tool that would perform the necessary steps to physically implement the circuit in silicon.

## Problem of Logic Synthesis[^2]

After placement and routing, the physical effects associated with wiring (e.g., increased signal delay and voltage drop) along with parasitic effects due to the proximity of certain devices cause **the “physical” version of the circuit to behave differently than the “logical” version of the circuit**. The result is <u>many iterations</u> between logic synthesis and place-and-route to achieve a working design.

## Why Physical Synthesis[^2]

Physical synthesis takes these implementation effects into consideration. An early floorplan of the design along with **detailed routing estimates** helps to provide early data on the previously mentioned physical effects. As a result, physical synthesis provides a convergent design flow that requires far fewer iterations.

The overall goal of physical synthesis is to <u>consider late-stage implementation effects early</u> in the design process with sufficient detail to create a convergent design flow. Early design results then fit the requirements of late-stage implementation with minimal need for re-work. The general process of considering late-stage impacts earlier is called a **shift-left** approach.

## Some Ideas[^1]

![](https://qph.fs.quoracdn.net/main-qimg-25e501c795ec5594d724655fe9cae8cb)



The logical and physical synthesis serve different targets in the RTL logic to netlist conversion.

- The constraints(SDC) are more mature in a physical synthesis. The logical synthesized netlist and floorplan are the inputs for a physical synthesis flow. So the area and placement of cells will be more accurate after physical synthesis.
- The logical synthesis optimizes the logic, timing and functionality implementation using minimum gates and DRV.
- The physical synthesis optimises the area, power, clock and power gating, scan optimisation along with constraints in logical synthesis.
- The target of physical synthesis is to achieve the minimum area usage at the required speed for a design. And for logical synthesis is timing with no functionality differences.

# Reference

[^1]: What is difference between logical synthesis and physical synthesis? https://www.quora.com/What-is-difference-between-logical-synthesis-and-physical-synthesis
[^2]: What is Physical Synthesis? https://www.synopsys.com/glossary/what-is-physical-synthesis.html
