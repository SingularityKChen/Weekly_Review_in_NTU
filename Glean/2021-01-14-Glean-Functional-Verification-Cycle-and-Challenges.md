---
layout: post
title: "[Glean] Functional Verification Cycle and Challenges"
description: "This post introduces the four phases in the functional verification cycle and its four challenges to reduce time and improve robustness at each stage. The corresponding solutions are mentioned as well, which can be seen as the suitable situations for different verification methodology."
categories: [Glean]
tags: [verification]
last_updated: 2021-01-14 12:02:00 GMT+8
excerpt: "This post introduces the four phases in the functional verification cycle and its four challenges to reduce time and improve robustness at each stage. The corresponding solutions are mentioned as well, which can be seen as the suitable situations for different verification methodology."
redirect_from:
  - /2021/01/14/
---

* Kramdown table of contents
{:toc .toc}
# Functional Verification Cycle and Challenges[^1]

## Functional verification cycle

As the graph shows, the functional verification cycle consists of four phases:

1. **Development**: verification plan, DV architecture, testbench, and tests development.
2. **Simulation**: software simulation, acceleration, emulation, etc. 
3. **Debug**: transaction level, signal level, etc. This will be a big component if you did not deploy assertions, for example.
4. **Cover**: functional, code, and SVA coverage that feeds back to the development
stage.

![functional verification cycle](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20210114113643.png)

## The challenge of reducing time and improving robustness at each stage

We need to reduce the time to complete and improve efficiency and robustness of the tasks at
each stage:

1. Reduce time to develop and improve robustness. 
2. Reduce time to simulate and improve simulation accuracy and throughput.
3. Reduce time to debug and improve efficiency. 
4. Reduce time to “comprehensive” cover.

### Reduce time to develop

Development time includes **functional verification plan development, verification environment creation, DV architecture development, testbench development, and tests development.**

Here is a strategy to reduce time to develop:

1. **Raise abstraction level of tests.** Use <u>TLM (Transaction Level Modeling) methodologies</u> such as UVM, SystemVerilog/C++/DPI, etc. The higher the abstraction level, the easier it is to model and maintain verification logic. Modification and debug of transaction level logic is much easier, further reducing time to develop testbench, reference models (scoreboard), peripheral models, and other such verification logic.
2. **Use constrained random verification** (CRV) methodologies to reach exhaustive coverage with fewer tests. Fewer tests mean less time to develop and debug.
3. **Develop verification components (e.g., UVM agents) that are reusable.** Make them parameterized for adoptability in future projects.
4. **Use SystemVerilog Assertions (SVA) to reduce time to develop complex sequential and combinatorial checks**. Verilog code for a given assertion will be much lengthier, hard to model, and hard to debug. SVA indeed reduces time to develop and debug.

### Reduce time to simulate

+ Higher-level abstractions simulate much faster than pure RTL testbench which is modeled at signal level. **Use transaction level testbench** (e.g., UVM, TLM 2.0 transaction level models).
+ **Deploy well-thought-out hardware acceleration, emulation, or FPGA prototype methodologies.** Develop transaction level testbenches that interact directly with
  the accelerated or emulated design.
+ **Use coverage-driven verification (CDV) methodologies** to reduce the number of tests to simulate to reach the defined coverage goals.

### Reduce time to debug

+ **Use SystemVerilog Assertion-based verification (ABV) methodology** to quickly reach to the source of the bug. Traditional way of debug is at IO level. In contrast, an SVA points directly at the source of the failure (e.g., a FIFO overflow assertion will point directly to the FIFO overflow logic in RTL that failed) drastically reducing the debug effort.
+ **Use transaction level methodologies** to reduce debugging effort (and not get bogged down into signal level granularity).
+ **Constrained random verification** allows for fewer tests. They also narrow down the cone of logic to debug.

### Reduce time to cover

+ Use **SystemVerilog functional coverage language** to measure the intent of the design. How well have your testbench verified the “intent” of the design. *For example, have you verified all transition of write/read/snoop on the bus? Have you verified that a CPU1-snoop occurs to the same line while a CP2-write invalid occurs to the same line?* Code coverage will not help with this.
+ **Use cover feature of SystemVerilog Assertions to cover complex sequential domain specification of your design.** “cover” helps with making sure that you have exercised low-level sequential domain conditions with your testbench. If an assertion does not fire, that does not necessarily mean that there is no bug. *One of the reasons for an assertion to not fire is that you probably never stimulated the required condition (antecedent) in the first place.*
+ **Use code coverage to cover structural coverage.** Structural coverage does not verify the intent of the design, it simply sees that the code that you have written has been exercised *(e.g., have you verified all “case” items of a “case” statement, or toggled all possible assigns, conditional expressions, states, etc.)*.

[^1]: Mehta, Ashok B. "ASIC/SoC functional design verification." *Publ. Springer* (2018).