---
layout: post
title: "[Glean] Static Sign-Off, Formal & Simulation"
description: "This post introduces the differences of Static Sign-Off, Formal and Simulation by three key functional verification metrics. analysis always finishes, all the violations flagged by the analysis, 100% of the failures are found."
categories: [Glean]
tags: [Signoff, Formal, Simulation]
last_updated: 2021-02-01 22:31:00 GMT+8
excerpt: "This post introduces the differences of Static Sign-Off, Formal and Simulation by three key functional verification metrics. analysis always finishes, all the violations flagged by the analysis, 100% of the failures are found."
redirect_from:
  - /2021/02/01/
---

* Kramdown table of contents
{:toc .toc}
# Static Sign-Off, Formal & Simulation[^1]

## Static Sign-Off, Formal & Simulation– Overlap & Differences

![Static-Sign-Off-vs-Formal-vs-Simulation](https://i1lur1j30fxivkpqmcddw1de-wpengine.netdna-ssl.com/wp-content/uploads/2019/07/Static-Sign-Off-vs-Formal-vs-Simulation-DAC19.jpg)

If you look at formal and simulation, they are both generic applications. That means <u>the user must build error checking</u>. To facilitate the error checking, they can buy VIPs or apps; but it’s the user’s responsibility to build the error checking.

Formal and static sign-off are <u>both static methods</u>; however, static sign-off includes <u>complete and customized error checking</u> in the context of the tool; as a result, the debugging is also customized to the application at hand.

## Simulation, Formal & Static Sign-Off Performance Against Verification Metrics

### Three Key Functional Verification Metrics

Here’s a very simplified model of how we can evaluate the merits of each of these three approaches.

![3-Verification-Methodology-Metrics-Ideal](https://i1lur1j30fxivkpqmcddw1de-wpengine.netdna-ssl.com/wp-content/uploads/2019/07/3-Verification-Methodology-Metrics-Ideal-DAC19.jpg)

1. The first metric is that the analysis always finishes, which means that the method <u>completes in a practical time frame</u>.
2. The metric on the left says that all the violations flagged by the analysis are <u>definitely design failures</u>.
3. The third metric says that, for the targeted checks, 100% of the failures are found. This is another way of saying that you can prove <u>the absence of any errors in the design</u>.

These are the three dimensions on which we can evaluate the various methodologies.

### Simulation

![Simulation-3-Verification-Metrics-DAC19](https://i1lur1j30fxivkpqmcddw1de-wpengine.netdna-ssl.com/wp-content/uploads/2019/07/Simulation-3-Verification-Metrics-DAC19.jpg)

When you create a test bench, you have some idea of how much time it will take for simulation to run, and that it will finish.

We also know that for simulation the failures that are flagged are definite failures.

However, simulation is the weakest on the third metric; it cannot confirm that 100% of failures have been detected.

The weakness of a methodology is where the engineering effort must be spent to overcome that deficiency.

### Formal

![Formal-3-Verification-Metrics](https://i1lur1j30fxivkpqmcddw1de-wpengine.netdna-ssl.com/wp-content/uploads/2019/07/Formal-3-Verification-Metrics-DAC19-1.jpg)

We know that in formal analysis, if a failure is flagged, it’s a definite failure. At the same time formal analysis is very capable of finding 100% of the failures; that is to say, it is capable of proving correctness.

But it lacks on the third dimension, which is the performance dimension; <u>the formal analysis may not finish in the project timeline.</u>

As a result, all the engineering effort goes into figuring out how to improve the completion rate for the formal analysis. This is the weakest dimension for formal analysis.

### Static Sign-Off

![Static-Sign-Off-3-Verification-Metrics](https://i1lur1j30fxivkpqmcddw1de-wpengine.netdna-ssl.com/wp-content/uploads/2019/07/Static-Sign-Off-3-Verification-Metrics-DAC19.jpg)

Static sign-off analysis finishes. Static sign-off also finds 100% of the failures that are targeted by the checks.

However, <u>static sign-off does not find definite failures — it finds potential failures, or the so-called “noise”</u>. The noise level for the static sign-off tools will vary from tool to tool; the accuracy will also vary from product to product.

It makes a trade-off in the third dimension. The idea is that if you can iterate and clean out all potential violations that are reported then you have sign-off.

[^1]: Verification Metrics for Simulation & Formal, & Static Sign-Off, https://www.realintent.com/verification-metrics-for-simulation-formal-static-sign-off/