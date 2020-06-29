---
layout: post
title: "[Read Paper] A Survey on Methods and Theories of Quantized Neural Networks"
description: "Reading note of A Survey on Methods and Theories of Quantized Neural Networks"
categories: [ReadPaper]
tags: [QNN, DNN, Algorithm]
last_updated: 2020-03-15 11:08:00 GMT+8
excerpt: 
redirect_from:
  - /2020/01/05/
---

* Kramdown table of contents
{:toc .toc}

# A Survey on Methods and Theories of Quantized Neural Networks

## Quantization Techniques

### Deterministic quantization

![Summarization of different quantization methods](https://images-cdn.shimo.im/E83E8CAU8qoHf9i7/image.png)

+ Rounding 

![rounding function](https://images-cdn.shimo.im/58vtJu1ZLs8MoiXT/image.png)

+ Vector Quantization: cluster the weights into groups and use the centroid of each group to replace the actual weights during inference.

  ![](https://images-cdn.shimo.im/tYIO1b2acTYLsA2f/image.png)

  	+ k-means
  	+ Hessian weighted k-means
  	+ product quantization
  	+ residual quantization

+ Quantization as Optimization: formulating the quantization problem as an optimization problem

### Stochastic Quantization

+ Random Rounding: In random rounding, the real value has no one-to-one mapping to the quantized value. Typically, the quantized value is sampled from a discrete distribution which is parameterized by the real values.

![random rounding function](https://images-cdn.shimo.im/Nj7hgrxuuyIB8VSC/image.png)

+ Probabilistic Quantization: the weights are assumed to be discretely distributed.

## Two Quantization Methodologies

![Summarization of fixed codebook and adaptive codebook quantization](https://images-cdn.shimo.im/Lcm0g5YaotI8sPTx/image.png)

### Fixed codebook quantization

The codebook is predefined.

### Adaptive Codebook Quantization

The codebook is learned from the data. Vector quantization and probabilistic quantization are two possible methods to achieve adaptive codebook quantization.

Two kinds of adaptive codebook quantization exist: hard quantization and soft quantization. 

+ In hard quantization, the real value is assigned exactly to be one of the discrete values. 
+ In soft quantization the real value is assigned to be some discrete value according to a probability distribution.

## Discussion

Quantized weights and activations occupy less memory compared with full-precision counterparts. Meanwhile, the training and inference speed can be greatly accelerated since the dot products
between weights and activations can be replaced by bitwise operations. Quantized gradients can reduce the overhead of gradient synchronization in parallel neural network training. In a single worker scenario, quantized gradients can accelerate back-propagation training as well as requiring less memory.

## Why Does Quantization Work?

They found that the binarization operation preserves some important properties of the full-precision networks, i.e,

+ Angle Preservation Property
+ Dot Product Proportionality Property

## Future of Quantized Neural Networks

The following possible directions for the next steps:

+ Develop more sophisticated rounding mechanism to train quantized neural network from scratch. One possible approach is to use the structure information of the weights to guide the rounding process.
+ Design quantized neural networks for tasks such as natural language processing, speech recognition and so on. Due to the varieties of deep learning models, a generally applicable quantization method is necessary.
+ Develop theoretical guidance for quantizing neural networks.