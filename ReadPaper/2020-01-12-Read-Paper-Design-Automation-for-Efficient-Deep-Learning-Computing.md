---
layout: post
title: "[Read Paper] Design Automation for Efficient Deep Learning Computing"
description: "Reading note of Design Automation for Efficient Deep Learning Computing"
categories: [ReadPaper]
tags: [DLA, DesignAutomation, NAS]
last_updated: 2020-03-07 12:29:00 GMT+8
excerpt: "Han Song's Paper, Design Automation, NAS, contains automatically designing specialized fast models, auto channel pruning, and auto mixed-precision quantization."
redirect_from:
  - /2020/01/12/
---

* Kramdown table of contents
{:toc .toc}

# Design Automation for Efficient Deep Learning Computing

![Design automation for model specialization, channel pruning and mixed-precision quantization](https://images-cdn.shimo.im/anETjNuh440NbKou/image.png)

Three main points in this paper:

+ automatically designing specialized fast models
+ auto channel pruning
+ auto mixed-precision quantization

## Automated Model Specialization

To fully utilize the hardware resource, start with a large design space (Figure 1(a)) that includes many candidate paths to learn which is the best one by gradient descent, rather than hand-picking with rule-based heuristics.

The search space for each block $i$ consists of many choices:

+ `ConvOp`: mobile inverted bottleneck conv [9] with various kernel sizes and expansion ratios
  + Kernel size: {3\*3, 5\*5, 7\*7}
    + Expansion ratio: {3, 6}
+ `ZeroOp`: if `ZeroOp` is chosen at $i^{th}$ block, it means the block is skipped.

In the forward step, to save GPU memory, we allow only one candidate path to actively reside in the GPU memory. This is achieved by hard-thresholding the probability of each candidate path to either 0 or 1 (i.e., path-level binarization).

## Automated Channel Pruning

Pruning too much will hurt accuracy; too less will not achieve high compression ratio.

Our automatic model compression (AMC) leverages reinforcement learning to efficiently search the pruning ratio.

We train a reinforcement learning agent to predict the best sparsity for a given hardware. We evaluate the accuracy and FLOPs after pruning. Then we update the agent by encouraging smaller, faster and more accurate models.

The easiest way to reduce the channels of a model is to use uniform channel shrinkage, i.e. use a width multiplier to uniformly reduce the channels of each layer with a fixed ratio.

## Automated Mixed-Precision Quantization

Our hardware-aware automatic quantization (HAQ) models the quantization task as a reinforcement learning problem. We use the actor-critic model to give the quantization policy (#bits per layer) (Figure 1(c)). The goal is not only high accuracy but also low energy and low latency.

Inferencing on edge devices and cloud servers can be quite different, since:

+ the batch size on the cloud servers are larger
+ the edge devices are usually limited to low computation resources and memory bandwidth.

Interpret the quantization policy’s difference between edge and cloud by the roofline model.