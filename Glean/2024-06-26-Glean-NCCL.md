---
layout: post
title: "[Glean] NVIDIA NCCL Introduction"
description: "This post introduces NVIDIA Collective Communication Library (NCCL)."
categories: [Glean]
tags: [NCCL, NVIDIA]
last_updated: 2024-06-27 11:17:00 GMT+8
excerpt: "This post introduces NVIDIA Collective Communication Library (NCCL)."
redirect_from:
  - /2024/06/26/
---

* Kramdown table of contents
{:toc .toc}

# NCCL

NVIDIA Collective Communication Library (NCCL) is optimized primitives for inter-GPU communication.[^1]

## Introduction

The NVIDIA Collective Communications Library (NCCL, pronounced “Nickel”) is a library providing inter-GPU communication primitives that are **topology-aware** and can be easily integrated into applications.[^2]

NCCL implements both **collective communication** and **point-to-point send/receive primitives**. It is not a full-blown parallel programming framework; rather, it is a library focused on accelerating inter-GPU communication.

NCCL provides the following collective communication primitives :

+ AllReduce
+ Broadcast
+ Reduce
+ AllGather
+ ReduceScatter

Additionally, it allows for point-to-point send/receive communication which allows for scatter, gather, or all-to-all operations.

## Reference

[^2]: [NVIDIA Collective Communication Library (NCCL) Documentation](https://docs.nvidia.com/deeplearning/nccl/user-guide/docs/index.html)
[^1]: [GitHub - NCCL](https://github.com/nvidia/nccl)
+ [NCCL: High-Speed Inter-GPU Communication for Large-Scale Training](https://www.nvidia.com/en-us/on-demand/session/gtcspring21-s31880/)
+ [Massively Scale Your Deep Learning Training with NCCL 2.4](https://developer.nvidia.com/blog/massively-scale-deep-learning-training-nccl-2-4/)
