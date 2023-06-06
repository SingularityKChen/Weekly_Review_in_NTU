---
layout: post
title: "[Glean] Two terms for timing analysis: WNS and TNS"
description: "WNS (worst negative slack) and TNS (total negative slack), including a summary table from ChatGPT4."
categories: [Glean]
tags: [Timing]
last_updated: 2023-04-05 09:30:00 GMT+8
excerpt: "WNS (worst negative slack) and TNS (total negative slack), including a summary table from ChatGPT4."
redirect_from:
  - /2022/10/04/
---

* Kramdown table of contents
{:toc .toc}

# Two Terms for Timing Analysis: WNS and TNS

WNS: worst negative slack, the worst slack value, characterizes the worst performance of the chip.

TNS: total negative slack, the sum of all negative slack values, represents a performance range of the chip.

It is an index to measure the degree of timing, usually a negative number. The more negative, the worse the performance.

# By ChatGPT4 -- WNS (Worst Negative Slack) and TNS (Total Negative Slack)

<!--Please explain the definition, and any other details of WNS (worst negative slack) and TNS (total negative slack) to me, as an experienced circuit architect, should understand and know about in the Markdown document including a summary table for me to establish a knowledge wiki.-->

WNS and TNS are critical metrics used to evaluate the timing performance of digital circuits. They help circuit architects identify timing violations and make necessary adjustments to optimize the design.

## Worst Negative Slack (WNS)

**Definition:** Worst Negative Slack (WNS) is the most significant negative slack value found across all timing paths in a digital circuit. It indicates the maximum extent to which the circuit's timing constraints are violated.

Negative slack occurs when the actual delay of a path exceeds the required delay, causing timing violations. WNS is used to identify the most critical timing violations in a design, which can help prioritize optimization efforts.

## Total Negative Slack (TNS)

**Definition:** Total Negative Slack (TNS) is the sum of all negative slack values across all timing paths in a digital circuit. It represents the cumulative timing violations in the design.

A high TNS value indicates multiple timing violations in the design, while a low TNS value (ideally zero) signifies a well-timed circuit. Analyzing TNS can help circuit architects identify the overall extent of timing issues and make informed decisions about design optimization.

## Summary Table

| Metric       | Definition                                                    | Use                                                                                     |
|--------------|---------------------------------------------------------------|-----------------------------------------------------------------------------------------|
| WNS          | The most significant negative slack value across all paths.   | To identify the most critical timing violations and prioritize optimization efforts.   |
| TNS          | The sum of all negative slack values across all paths.        | To evaluate the overall extent of timing violations and guide design optimization.     |

## Importance for Circuit Architects

As an experienced circuit architect, understanding WNS and TNS is essential for the following reasons:

1. **Identify Timing Violations:** WNS and TNS help identify timing violations in the design, which can impact the overall performance and reliability of the circuit.
2. **Optimization Prioritization:** WNS assists in determining which timing paths need the most attention, allowing for efficient design optimization.
3. **Design Analysis:** TNS provides a comprehensive view of the overall timing performance, aiding in the evaluation of design trade-offs.
4. **Iterative Improvement:** By monitoring changes in WNS and TNS values, architects can assess the effectiveness of design changes and guide iterative improvements.

Understanding and utilizing WNS and TNS will enable you to optimize your circuit designs effectively and ensure reliable performance in the end product.

