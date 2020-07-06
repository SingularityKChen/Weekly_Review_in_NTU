---
layout: post
title: "[Read Paper] TEA-DNN: the Quest for Time-Energy-Accuracy Co-optimized Deep Neural Networks"
description: "Reading note of TEA-DNN: the Quest for Time-Energy-Accuracy Co-optimized Deep Neural Networks"
categories: [ReadPaper]
tags: [DNN, ParetoOptimal]
last_updated: 2020-07-05 10:07:45 GMT+8
excerpt: "This paper introduces a method to optimize DNN with Pareto-optimal models and Bayesian optimization."
redirect_from:
  - /2020/06/28/
---

* Kramdown table of contents
{:toc .toc}
# TEA-DNN the Quest for Time-Energy-Accuracy Co-optimized Deep Neural Networks

## Solved problem

+ NAS with considering the available hardware resources
+ Leverage energy and execution time
+ To my understanding:
  + Find an optimal CNN structure for one target hardware platform

Assume that classification error is not affected by the specific hardware a network is run on

## The main idea and methods
- Formulate the neural architecture search problem as a multi-objective optimization problem
- Leverage Bayesian optimization to search for Pareto-optimal solutions
- Directly measure the real-world values for all the three objectives (i.e., time, energy and accuracy)
  - Why: eliminates the need to model the targeted hardware

## Background knowledge

+ Pareto-optimal models
+ Bayesian optimization

## TEA-DNN Optimization Framework

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200629224830.png" style="zoom:50%;" />

## Network Architecture Predefined

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200629224851.png" style="zoom:50%;" />

## The Search Space

+ The input space of each building block consists of the **outputs of all preceding blocks** in the current cell as well as **outputs from the two preceding cells**
+ The operation space includes the following eight functions commonly used in top performing CNNs: 
  1. max 3 × 3: 3 × 3 max pooling 
  2. identity: identity mapping 
  3. sep 3 × 3: 3 × 3 depthwise-separable convolution 
  4. conv 3 × 3: 3 × 3 convolution 
  5. sep 5 × 5: 5 × 5 depthwise-separable convolution 
  6. conv 5 × 5: 5 × 5 convolution 
  7. sep 7 × 7: 7 × 7 depthwise-separable convolution 
  8. conv 7 × 7: 7 × 7 convolution