---
layout: post
title: "[Glean] Formal Signoff"
description: "This post introduces VC Formal Apps, the reason and goal of formal signoff. Later seven steps of formal signoff based on Synopsys are listed."
categories: [Glean]
tags: [Signoff, Formal, Verification]
last_updated: 2021-02-02 10:01:00 GMT+8
excerpt: "This post introduces VC Formal Apps, the reason and goal of formal signoff. Later seven steps of formal signoff based on Synopsys are listed."
redirect_from:
  - /2021/01/29/
---

* Kramdown table of contents
{:toc .toc}
# Formal Signoff

## VC Formal Apps[^3]

![VC Formal Apps](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20210201222347.png)

![VC Formal Apps Types](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20210201222436.png)

## Introduction

Each stage in the development process must have clearly defined criteria for its contribution to final signoff. Formal verification is no exception.

Formal verification involves mathematical analysis of the register transfer level (RTL) design and user-specified properties about the design. Assert properties (assertions) specify intended design behavior, assume properties (constraints) control the analysis, and cover properties measure how well the analysis exercises the design.[^1]

## Why do we need and what is the goal of formal signoff

It is a powerful technology, but it is important for users to keep in mind that **the results are only as good as the properties being analyzed.** Formal is exhaustive but only with respect to what you write![^2]

Initial FV approach did not have a well defined closure criteria 

- Properties developed

- Designer reviewed properties
- Human analysis of bounded depths

The ultimate goal of formal signoff is to ensure that “what you write” does in fact cover all the scenarios you expected[^2]

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20210129210525.png)

## Formal Signoff Criteria - Functional Correctness[^3]

![Formal Signoff Criteria](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20210201223007.png)

## Seven steps of formal signoff[^1][^2]

### Review test plan and specification

The first step is a thorough review of the test plan and the specification. Design and verification engineers write the properties based on design features identified in the test plan. If the plan does not accurately reflect the intended functionality in the spec, then the properties are likely to be incomplete.

This review is typically performed by **the verification team**, although **design engineers** may be involved as well.

### Review the properties and the formal results

**Designers** are required for the second step. They know their own RTL code and the intended behavior, so have deep insight into which aspects of the design should be checked by assertions and covered during verification.

### Bounded depth analysis

Has the design been analyzed at sufficient sequential depth?

VC Formal applies algorithms that can generate bounded proofs and reports **the number of cycles (depth) for each such proof**. It also provides information on the sequential depth of the overall design to help the user decide whether the achieved bounded proof provides high confidence (though not certainty) that the design is correct. 

1. Generate cover points within COI of property
2. Analyze cover points using shortest path formal engines
3. Maximum depth reached gives a rough idea of the sequential depth of the property – any number less than this shows lack of required exploration

### Structural property cone of influence analysis (COI, Property Density)

Are there enough properties to check and cover all the design?

Analyzing the registers within the structural COI of all the assertions.

Finds holes in verification quickly, but due to structural nature can miss holes.

![Quickly tracing the cones of influence.](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20210129211342.png)

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20210201223455.png)

<small>This graph from [^3]</small>

### Over constraint reachability analysis

Are there any invalid proofs due to incorrect constraints?

The fourth step addresses the concern that **over-constraining may mask errors in the design**. If the constraints are incorrect, then formal analysis might not be considering some of the legal design behavior.

The analysis reports exactly which constraints affect which parts of the design, showing the root cause of the unreachability and making it much easier for the user to perform the necessary adjustments.

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20210202092734.png)

<small>This graph from [^3]</small>

### Formally generated core of driving logic on properties (Formal Core)

Which portions of the design have truly been verified?

This step tackles the question of which portions of the design have truly been verified.

The COI approach is fast but overly optimistic in judging assertion density and quality. Some registers or inputs within a COI may not actually be involved in the proof (full or bounded) of the related assertion.

Formal Core is Stronger than COI:

+ Formal Core indicates which signals are involved in the proof of an assertion
+ But does not show testing of everything about a register[^3]

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20210202093111.png)

<small>This graph from [^3]</small>

#### Simulation v.s. Formal Core Analysis[^3]

![Simulation v.s. Formal Core Analysis](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20210202093540.png)

### Automated fault injection assertion stress testing (FTA)

#### Introduction to FTA

Can the assertions catch all possible bugs in the design?

The final step combines VC Formal with the Synopsys Formal Testbench Analyzer (FTA) app to provide the most precise assertion metrics. The FTA app inserts faults (mutations) into the design with Synopsys Certitude functional qualification system to model the types of bugs typically found in RTL code.

It analyze the results of properties in the presence of artificially inserted faults. If at least one property fails in presence of a fault then assertion is good, if none fail then indication of verification holes.

FTA is stronger than Formal Core:

+ FTA checks whether assertions can catch injected faults
+ Formal Core checks that **something** about a register is checked
+ FTA checks whether **other features** of that logic are checked

![Performing formal core analysis and providing coverage results.](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20210129212117.png)

#### Fault Types[^3]

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20210202093830.png)

#### Pro

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20210202094240.png)

## Summary

![Summary](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20210202094644.png)


[^1]: The Seven Steps Of Formal Signoff, https://semiengineering.com/the-seven-steps-of-formal-signoff/
[^2]: The Importance of Complete Signoff Methodology for Formal Verification, https://2020.dvcon-virtual.org/sites/dvcon20/files/2020-05/01_1_P.pdf
[^3]: A Step-by-Step Approach to Formal Signoff, https://readytalk.webcasts.com/starthere.jsp?ei=1296788&tp_key=aa0b321b95&sti=web