---
layout: post
title: "[Glean] CUDA C++ Maximize Memory Throughput"
description: "This post introduces how to maximize memory throughput in CUDA C++ programming."
categories: [Glean]
tags: [CUDA, NVIDIA]
last_updated: 2024-06-27 11:17:00 GMT+8
excerpt: "This post introduces how to maximize memory throughput in CUDA C++ programming."
redirect_from:
  - /2024/04/02/
---

* Kramdown table of contents
{:toc .toc}

# CUDA C++ Maximize Memory Throughput[^1]

The first step in maximizing overall memory throughput for the application is to minimize data transfers with low bandwidth.

That means minimizing data transfers between the host and the device since these have much lower bandwidth than data transfers between global memory and the device.

That also means minimizing data transfers between global memory and the device by maximizing use of on-chip memory: **shared memory and caches** (*i.e.*, L1 cache and L2 cache available on devices of compute capability 2.x and higher, texture cache and constant cache available on all devices).

Shared memory is equivalent to a user-managed cache: The application explicitly allocates and accesses it.
As illustrated in CUDA Runtime, a typical programming pattern is to **stage data coming from device memory into shared memory**; in other words, to have each thread of a block:

- Load data from device memory to shared memory,
- Synchronize with all the other threads of the block so that each thread can safely read shared memory locations that were populated by different threads,
- Process the data in shared memory,
- Synchronize again if necessary to make sure that shared memory has been updated with the results,
- Write the results back to device memory.

# Reference

[^1]: [CUDA C++ Programming Guide 12.4](https://docs.nvidia.com/cuda/cuda-c-programming-guide/#maximize-memory-throughput)
