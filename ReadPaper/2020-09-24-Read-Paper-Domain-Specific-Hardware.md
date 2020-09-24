---
layout: post
title: "[Read Paper] Domain-Specific Hardware"
description: "Reading note of Domain-Specific Hardware"
categories: [ReadPaper]
tags: [DSA]
last_updated: 2020-09-24 21:06:00 GMT+8
excerpt: "Some tricks for design domain specific accelerators."
redirect_from:
  - /2020/09/24/
---

* Kramdown table of contents
{:toc .toc}
# Sources of Acceleration

## Data Specialization

The defining feature of many domain-specific accelerators is a set of hardware operations specialized to the application domain.

In many cases, specialized logic can perform the entire inner loop **in a single cycle** with a small amount of area and power. This logic is fed by **specialized registers** and **communication links** that provide and consume data with very low energy.

Much of the 81nJ consumed by the x86 processor and much of its area are **spent fetching, decoding, and reordering instructions**. This overhead is largely eliminated by specialization.

Specialization also enhances locality by reducing the cost of memory compression. 

The compressed-sparse representation of network weights results in a 30× reduction in size allowing the weights of most networks to fit into **efficient, local, on-chip memories**, which takes two orders of magnitude less energy to access than off-chip memories.

## Parallelism

Most domain-specific accelerators exploit parallelism at one or more levels.

As an example, the alignment portion of our Darwin accelerator exploits parallelism at two levels.

At the outer-loop level, A = 64 systolic arrays of processing elements process 64 separate alignment problems in parallel. **There is no communication between the subproblems**, and the only synchronization required is upon completion of each subproblem.

At the inner-loop level, each array consists of P = 64 processing elements that compute 64 elements of the H, I, and D matrices in parallel.

Only systolic nearest neighbor communication between the processing elements is required. As with all systolic arrays, synchronization is implicit.

## Local and Optimized Memory

The gains from specialization and parallelism are dependent on keeping the computation supplied from small, local memories.

Data compression can be employed to both increase the effective size of a local memory and to increase the effective bandwidth of a memory interface.

## Reduced Overhead

Even a simple in-order processor spends over 90% of its energy on overhead: instruction fetch, instruction decode, data supply, and control.

A modern out-of-order processor spends over 99.9% of its energy on overhead adding costs for branch prediction, speculation, register renaming, and instruction scheduling.

Performing a 32-b integer add takes only 63 fJ in 28nm CMOS. Performing an integer add instruction on a 28nm ARM A-15 takes over 250pJ, about 4000× the energy of the add itself. There are no instructions to be fetched and hence no instructions fetch and decode energy.

Moreover, most adds do not need full 32-bit precision and just the number of bits needed are added, further saving energy. 

# Co-Design Is Needed

Because existing algorithms are **highly tuned for conventional general-purpose processors**, they are rarely the optimal approach for a specialized solution. Instead, the algorithm and hardware must be co-designed to jointly optimize performance and efficiency while preserving or enhancing accuracy.

Many existing algorithms are tuned to balance the performance of conventional processors with their memory systems.

To get significant speedup, such algorithms must be refactored to **reduce the bandwidth demands on global memory**. Although methods such as **tiling and compression** can be used to reduce global bandwidth to some degree, often more fundamental restructuring is required.

One approach to co-design is to trade more of an operation that is inexpensive in hardware (that is, **logic limited**) for less of an operation that is expensive (that is, **memory limited**).

Co-design may also be used to reduce memory footprint—to make the use of **small local memories feasible**.

Co-designing special-purpose hardware for sparse operations enables these algorithms to be used to reduce computation as well.

# Memory Dominates Accelerators

The area and power of most accelerators are dominated by memory, and their performance is often memory limited. As a result, much of the co-design described earlier is developing algorithms that have a small memory footprint.

Because the area and power of most accelerators are memory dominated, a reasonable first estimate of these costs **can be made by considering only the memory**. This allows rapid design space exploration.

Because global memory bandwidth is extremely expensive, many accelerators are designed to be global memory limited—**to keep this expensive resource busy**.

Because external memory bandwidth is so critical, it should be optimized. Memory schedulers should be employed that **maximize memory throughput** and memory contents should be compressed where possible.

# Balancing Specialization and Generality

In the design of domain-specific accelerators, there is a tension between generality and efficiency.

## Special Instructions vs. Special Engines

One approach to building accelerators for broad domains is to **add specialized instructions** to a general-purpose processor. A hardware block is built to accelerate the core operations for a domain of algorithms. This approach makes the core operation as efficient as a completely specialized accelerator but allows the use of the programmable general-purpose processor to adapt its use to different algorithms and applications.

The advantage of implementing the accelerator as an instruction is that the full power of the general-purpose processor is available to implement other layers of the network.

One advantage of building a specialized instruction rather than an entire engine is that only the instruction must be developed, not the entire system.

## Domain-specific Parallel Computers

MARS used a single domain-specific programmable processing element that could serve as any of the pipeline stages. A key factor in the success of MARS was the low-overhead associated with horizontal microcode control.

# Total Cost of Ownership (TCO)

The technology node used to implement an accelerator should be chosen to give minimum total cost of ownership (TCO).

# Accelerator Design

The design of a domain-specific accelerator is really **the design of a fine-grained, memory-constrained, parallel program for a limited set of tasks**. Most of the effort is in crafting an algorithm that achieves high parallelism with a small local memory footprint and low global memory bandwidth.

Global memory operations are hundreds of times more expensive than arithmetic operations and local memory operations—those that hit in the cache.

# Further Read

+ Cong, J., et al. Charm: A composable heterogeneous accelerator-rich microprocessor. In ISLPED (2012). ACM, 379–384.
+ Nickolls, J., Dally, W.J. The GPU computing era. IEEE Micro 30, 2 (2010), 56–69.
+ Turakhia, Y., Bejerano, G., Dally, W.J. Darwin: A genomics co-processor provides up to 15,000× acceleration on long read assembly. In ASPLOS (2018). ACM, 199–213.
+ Vasilakis, E. An instruction level energy characterization of arm processors. Tech. Rep. FORTHICS/TR-450. Foundation of Research and Technology Hellas, Institute of Computer Science, 2015.
+ Wolfe, M. Iteration space tiling for memory hierarchies. In Proceedings of the Third SIAM Conference on Parallel Processing for Scientific Computing (1987), Society for Industrial and Applied Mathematics, 357–361.