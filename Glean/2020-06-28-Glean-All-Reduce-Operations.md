---
layout: post
title: "[Glean] All-Reduce Operations"
description: "The all reduce operations are one kind of collective operations in NCCL and MPI lib."
categories: [Glean]
tags: [AllReduce]
last_updated: 2020-06-29 09:21:00 GMT+8
excerpt: "The all reduce operations are one kind of collective operations in NCCL and MPI lib."
redirect_from:
  - /2020/06/28/
---

* Kramdown table of contents
{:toc .toc}
# All-Reduce Operations

The all reduce operations are one kind of collective operations in NCCL[^1] and MPI[^2] lib.

> Many parallel applications will require accessing the reduced results across all processes rather than the root process. In a similar complementary style of `MPI_Allgather` to `MPI_Gather`, `MPI_Allreduce` will reduce the values and distribute the results to all processes.

![](https://mpitutorial.com/tutorials/mpi-reduce-and-allreduce/mpi_allreduce_1.png)

> The AllReduce operation is performing reductions on data (for  example, sum, max) across devices and writing the result in the receive  buffers of every rank.
>
> The AllReduce operation is rank-agnostic. Any reordering of the ranks will not affect the outcome of the operations.

![](https://docs.nvidia.com/deeplearning/nccl/user-guide/docs/_images/allreduce.png)

Here are also other collective operations in NCCL:

+ Broadcast
+ Reduce
+ All Gather
+ Reduce Scatter

[^1]: Operations - NCCL, https://docs.nvidia.com/deeplearning/nccl/user-guide/docs/usage/operations.html
[^2]: MPI Reduce and Allreduce, https://mpitutorial.com/tutorials/mpi-reduce-and-allreduce/