---
layout: post
title: "[Survey] MLforHPC Benchmarks "
description: "Survey: MLforHPC Benchmarks"
categories: [ReadPaper, Survey]
tags: [ML4HPC, HPML, HPC, Benchmark]
last_updated: 2020-03-07 11:00:00 GMT+8
excerpt: "I attached my recent survey on ML4HPC benchmarks, including three papers 1) A Modular Benchmarking Infrastructure for High-Performance and Reproducible Deep Learning; 2) HPC AI500: A Benchmark Suite for HPC AI Systems; 3) A Modular Benchmarking Infrastructure for High-Performance and Reproducible Deep Learning; and some other presentation slides."
redirect_from:
  - /2020/02/16/
---

* Kramdown table of contents
{:toc .toc}

# ML For HPC Benchmarks

The screenshot bellow shows the materials related to HPC benchmarks. Although most of them are `HPC for AI` instead of `AI for HPC`.

![Current Benchmarks](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200216190127CurrentBenchmark.png)


## [Implications of Integration of Deep Learning and HPC for Benchmarking](http://dsc.soic.indiana.edu/presentations/BenchCouncilNov19-2019.pptx)

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200216200055.png)

![ML4HPC & HPC4ML](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200216200426.png)

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200216200450.png)

## [HPC AI500: A Benchmark Suite for HPC AI Systems](http://arxiv.org/abs/1908.02607)

![HPC AI500 Methodology](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200216201801.png)

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200216201831.png)

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200216201855.png)

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200216201931.png)

## A Modular Benchmarking Infrastructure for High-Performance and Reproducible Deep Learning

![Components in Deep Learning](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200216202234.png)

This benchmark utilize the Open Neural Network Exchange format to store DNN reproducibly. And almost all frameworks support generate this kind of files. So this benchmark could combine them together.

The essence of Deep500 is its layered and modular design that allows to independently extend and benchmark DL procedures related to simple operators, whole neural networks, training schemes, and distributed training. The principles behind this design can be reused to enable interpretable and reproducible benchmarking of extreme-scale codes in domains outside DL.

### Benchmark Challenges

+ customizability: the ability to seamlessly and effortlessly combine different features of different frameworks and still be able to provide fair analysis of their performance and accuracy.
+ metrics: considering performance, correctness, and convergence in shared- as well as distributed-memory environments. 
  + some metrics can test the performance of both the whole computation and fine-grained elements, for example latency or overhead.
  + others, such as accuracy or bias, assess the quality of a given algorithm, its convergence, and its generalization towards previously-unseen data.
  + combine performance and accuracy (time-to-accuracy) to analyze the tradeoffs.
  + propose metrics for the distributed part of DL codes: communication volume and I/O latency.
+ performance and scalability: the design of distributed benchmarking of DL to ensure high performance and scalability.
+ validation: A benchmarking infrastructure for DL must allow to validate results with respect to several criteria. As we discuss in § V, Deep500 offers validation of convergence, correctness, accuracy, and performance.
+ reproducibility: the ability to reproduce or at least interpret results.

### Design and Implementation of Deep500

![The design of Deep500](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200216203704.png)

The core enabler in Deep500 is the modular design that groups all the required functionalities into four levels: 
+ “Operators”
+ “Network Processing”
+ “Training”
+ “Distributed Training”

Each level provides relevant abstractions, interfaces, reference implementations, validation procedures, and metrics.

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200216203935.png)
