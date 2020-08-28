---
layout: post
title: "[Read Paper] Code compression for embedded VLIW processors using variable-to-fixed coding"
description: "Reading note of Code compression for embedded VLIW processors using variable-to-fixed coding"
categories: [ReadPaper]
tags: [VLIW, RISC, Compression, Tunstall, V2F]
last_updated: 2020-08-28 16:00:00 GMT+8
excerpt: "In this paper, it introduces a compress method which uses variable-to-fixed coding schemes based on either Tunstall coding or arithmetic coding to overcome the communication bottleneck between memory and CPU, especially for RISC or VLIW processors, which have a "code size bloating" problem compare to CISC processors."
redirect_from:
  - /2020/08/26/
---

* Kramdown table of contents
{:toc .toc}
# Code compression for embedded VLIW processors using variable-to-fixed coding

In this paper, it introduces a compress method which uses variable-to-fixed coding schemes based on either Tunstall coding or arithmetic coding to overcome the communication bottleneck between memory and CPU, especially for RISC or VLIW processors, which have a "code size bloating" problem compare to CISC processors.

## Introduction

In many embedded systems, the cost of RAM or ROM often outweighs that of the main processors. Choosing a processor for an embedded system is sometimes determined by the code size, not performance, since the difference between one CPU’s object code and another can be as much as a 3:1 ratio.

Code size reduction is still one of the most important design goals for embedded systems due to the
following reasons.

+ Embedded system designers are adding more functions and creating more complex programs.
+ More and more embedded system programs are written in high-level languages, However, the compiler-generated code is often much larger than the hand-optimized assembly code.
+ Many embedded systems use reduced instruction set computer (RISC) or very long instruction word (VLIW) processors, which have a “code size bloating” problem compared to complex instruction set computer (CISC) processors.

### Code Size Reduction

There have been many efforts to reduce embedded system code size for RISC and VLIW architectures, and they can be categorized as the following.

+ Compiler Techniques: Register renaming, inter-procedural optimization, together with procedural abstraction of repeated code fragments. The compiler techniques have the advantage of avoiding any runtime decompression overheads and require no hardware change.
+ ISA Modification: This approach modifies or customizes the instruction set architecture to achieve code size reduction. This approach requires a considerable effort to design the instruction set and requires a new instruction decoder, a whole new set of software development tools, such as a specialized compiler, an assembler, and a linker.
+ Code Compression: It refers to compressing the executable program offline and decompress it
  on-the-fly during execution. This approach requires no change of compiler or processor architecture. It can also be combined together with the previous two approaches to achieve further code size
  reduction.

### Code Compression Background

A code compression/decompression scheme typically operates in a manner transparent to the processor and requires no change to such tools as compilers, linkers, and loaders.

#### Two Problems

Compressed codes must be decompressed during program execution. This implies that it is necessary to ensure random access during decompression, since branch, jump, and call instructions can alter the program execution flow. It is, therefore, necessary to split code into segments or blocks that can be decompressed individually. It is necessary to build a mapping between the original address space and the compressed address space.

Another problem associated with code compression is indexing the compressed blocks. If the main memory is compressed, the original program address will not store the expected instruction that the CPU needs. Therefore, it is necessary to build a mapping between the original address space and the compressed address space.

#### The Decompression Architecture

**Pre-cache Architecture**: The decompression engine is placed between the memory and the cache. The advantage of this architecture is that decompression is not time critical since it happens only when the instruction cache needs a refill. Compression does not improve the utilization of the cache since it is not
compressed.

**Post-cache Architecture**: The decompression engine is placed between the cache and the processor. decompression is on the critical path of the instruction execution pipeline, and a fast decompression unit design is critical.