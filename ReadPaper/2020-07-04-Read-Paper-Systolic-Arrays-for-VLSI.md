---
layout: post
title: "[Read Paper] Systolic Arrays for VLSI"
description: "Reading note of Systolic Arrays for VLSI"
categories: [ReadPaper]
tags: [Systolic, DLA]
last_updated: 2020-07-04 11:08:00 GMT+8
excerpt: "This paper proposes new multiprocessor structures and parallel algorithms for processing some basic matrix computations which are capable of pipelining matrix computations with optimal speed-up."
redirect_from:
  - /2020/07/04/
---

* Kramdown table of contents
{:toc .toc}
# Systolic Arrays for VLSI

This paper is also published as *Algorithms for VLSI Processor Arrays* at *introduction to VLSI systems*. It proposes new multiprocessor structures and parallel algorithms for processing some basic matrix computations which are capable of pipelining matrix computations with optimal speed-up.

The pipelining aspect of the algorithms is most effective for band matrices with long bands.

By performance, this paper holds the view that **time spent in I/O, control and data movement as well as arithmetic must all be considered.**

## The Basic Components and Array Structures

### The Inner Product Step Processor

The single operation considered is inner product step $R_C ← R_C + R_A × R_B$. But we can implementation other kinds of "multiplication" or "add".

This processor should makes the input values for $R_A$, $R_B$, and $R_C$, respectively; computes the inner product $R_C ← R_C + R_A × R_B$; and makes the inputs values for $R_A$, $R_B$, together with the new value of $R_C$ available as outputs on the output lines.

Here are two types of the inner product step processor.

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200704104257.png" alt="Geometries for the  inner product step processor" style="zoom:67%;" />

### Processor Arrays

The basic network organization we shall adopt for processors is **mesh-connected** and all connections from a processor are to **neighboring processors**.

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200704105023.png" alt="Processor Arrays" style="zoom:50%;" />

Processors lying on the boundary of the processor array may have external connections to the host memory.

## Matrix-Vector Multiplication

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200704105715.png" alt="Matrix-Vector Multiplication" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200704105608.png" alt="Linearly connected network for the Matrix-Vector Multiplication" style="zoom: 33%;" />

$w = p + q - 1$.

The utilization ratio is less than $50\%$ with $O(2n + w)$ time units, compared to $O(wn)$ time for the sequential algorithm on a uniprocessor.

## Matrix Multiplication on a Hexagonal Array

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200704110054.png" alt="Matrix Multiplication" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200704110121.png" alt="Matrix Multiplication on a Hexagonal Array" style="zoom:50%;" />

The utilization ratio is less than $1\over3$ with $O(3n + min(w_1, w_2))$ time units.

## The LU-Decomposition of a Matrix on a Hexagonal Array

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200704110743.png" alt="The LU-Decomposition of a Matrix" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200704110840.png" alt="The LU-Decomposition of a Matrix on a Hexagonal Array" style="zoom:50%;" />

It can compute in $O(3n + min(p, q))$ time. If $A$ is an $n × n$ dense matrix, then the computation time is $4n$.

## Triangular Linear Systems

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200704111212.png" alt="Triangular Linear Systems" style="zoom:50%;" />

![Triangular Linear Systems in linearly connected network](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200704111237.png)