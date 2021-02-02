---
layout: post
title: "[Glean] VC Formal Apps"
description: "This post introduces the Apps of VC formal, including AEP, FCA, CC, SEQ, FRV, FXP, FPV, FTA, FSV, DPV, RMA, AIP and FuSa."
categories: [Glean]
tags: [Formal, VC Formal]
last_updated: 2021-02-02 10:19:00 GMT+8
excerpt: "This post introduces the Apps of VC formal, including AEP, FCA, CC, SEQ, FRV, FXP, FPV, FTA, FSV, DPV, RMA, AIP and FuSa."
redirect_from:
  - /2021/02/02/
---

* Kramdown table of contents
{:toc .toc}
# VC Formal Apps[^1]

## Introduction

The VC Formal solution includes a comprehensive set of formal applications (Apps), including Formal Property Verification (FPV), Automatic Extracted Properties (AEP), Formal Coverage Analyzer (FCA), Connectivity Checking (CC), Sequential Equivalence Checking (SEQ), Formal Register Verification (FRV), Formal X-Propagation Verification (FXP), Formal Testbench Analyzer (FTA), Regression Mode Accelerator (RMA), Datapath Validation (DPV), Functional Safety (FuSa) and a portfolio of Assertion IPs (AIP) for verification of standard bus protocols.

## Productivity Apps

![Productivity Apps](https://www.synopsys.com/content/dam/synopsys/verification/diagrams/vc-formal-1.jpg)

### Automatic Extracted Property Checks (AEP)

Automatic functional analysis for <u>out of bound arrays, arithmetic  overflow, X-assignments, simultaneous set/reset, full case, parallel  case, multi driver/conflicting bus and floating bus checks</u> without the  need for dedicated simulation tests.

### Formal Coverage Analysis (FCA)

Complementing simulation flows, VC formal provides proof that <u>uncovered  points in coverage goals</u> are indeed unreachable, allowing them to be  removed from further analysis—saving significant manual effort.

### SoC-level Connectivity Checking (CC)

<u>Verification of connectivity at the SoC level</u>. Flexible input format  ensures ease of integration. Powerful debugging, including value  annotation, schematic viewing, source code browsing and analysis  reporting speeds analysis. Automatic root-cause analysis of unconnected  connectivity checks saves significant debug time.

### Sequential Equivalency Checking (SEQ)

This allows <u>comparison of designs, after register retiming, insertion of clock gating for power optimization or microarchitecture changes</u>.

### Formal Register Verification (FRV)

Helps to formally verify behavior of <u>configuration registers for respective attributes</u> like “read only”,  “read/write” or “reset value” eliminating the need for directed  simulation tests.

### Formal X-Propagation Verification (FXP)

Checks for <u>unknown signal value (X) propagation through the design</u> and  allows tracing of the failed property to source of X in the Verdi  schematic and waveform.

## High Value Apps

![High Value Apps](https://www.synopsys.com/content/dam/synopsys/verification/diagrams/vc-formal-2.jpg)

### Formal Assertion-Based Property Verification (FPV)

Formal proof-based techniques to <u>verify SystemVerilog Assertion (SVA)  properties</u> to ensure correct operation across all possible design  activity even before the simulation environment is available. Advanced  assertion visualization, property browsing, grouping and filtering allow simple concise access to results. Formal navigator enables debug and  "what-if-analysis" with only the RTL design in the Unified Verdi GUI  with/without assertions or Testbench. 

### Formal Testbench Analyzer (FTA)

Certitude™ provides the unique capability to assess <u>the quality of formal environment</u>. The native integration of Certitude with VC Formal provides meaningful <u>property coverage measurements</u> as part of formal signoff and identifies any weaknesses such as missing or incorrect properties or constraints. The native integration delivers 5-10X faster performance compared to stand-alone fault injection methods.

### Formal Security Verification (FSV)

Helps formally verify that secure data should not reach <u>non-secure destinations and ensures data integrity</u>, where non-secure data should not over-write (or reach) secure destinations.

### Datapath Validation (DPV)

Integrated HECTOR™ technology within VC Formal and contains custom optimizations and engines for <u>datapath verification (ALU, FPU, DSP etc.) using transaction level equivalence</u>. This app leverages the Verdi graphical user interface for debug.

### Regression Mode Accelerator (RMA)

Provides significant performance improvement to formal property verification <u>using Machine Learning technology</u>. Use of this app <u>accelerates formal property verification</u> to achieve better convergence of formal proofs for subsequent runs and enables significant saving of compute resources in nightly formal regressions.

### Assertion IP (AIP)

High performance and optimized assertion IP for verification of <u>standard bus protocols</u>, and usable in Synopsys VC Formal solution and VCS simulation.

### Functional Safety Verification (FuSa)

Functional safety verification is an essential requirement for automotive SoC and IP designs. This App formally <u>identifies and classifies faults based on observability or detectability criteria</u>. Complementing fault simulation with Z01X™, the combined solution reduces the effort and time required to achieve the test coverage closure. 

[^1]: VC Formal, https://www.synopsys.com/verification/static-and-formal-verification/vc-formal.html