---
layout: post
title: "[Read Paper] ProxylessNAS: Direct Neural Architecture Search on Target Task and Hardware"
description: "Reading note of ProxylessNAS: Direct Neural Architecture Search on Target Task and Hardware"
categories: [ReadPaper]
tags: [NAS, DLA]
last_updated: 2020-03-07 12:25:00 GMT+8
excerpt: "Han Song's Paper: ProxylessNAS. ProxylessNAS that can directly learn neural network architectures on the target task and target hardware without any proxy."
redirect_from:
  - /2020/01/12/
---

* Kramdown table of contents
{:toc .toc}

# ProxylessNAS: Direct Neural Architecture Search on Target Task and Hardware

ProxylessNAS is the first NAS algorithm that directly learns architectures on the largescale dataset (e.g. ImageNet) without any proxy while still allowing a large candidate set and removing the restriction of repeating blocks. it is the first work to study specialized neural network architectures for different hardware architectures.

![optimizes neural network architectures](https://images-cdn.shimo.im/qiQowKgCctgUyfLd/image.png)

## Method

+ describe the construction of the over-parameterized network with all candidate paths
+ introduce how we leverage binarized architecture parameters to reduce the memory consumption of training the over-parameterized network to the same level as regular training.

![Learning both weight parameters and binarized architecture parameters](https://images-cdn.shimo.im/GtQyHdVEM7UiZmh7/image.png)

+ first freeze the architecture parameters and stochastically sample binary gates according to Eq. (2) for each batch of input data
+ the weight parameters of active paths are updated via standard gradient descent on the training set (Figure 2 left)
+ When training architecture parameters, the weight parameters are frozen
+ reset the binary gates and update the architecture parameters on the validation set (Figure 2 right). These two update steps are performed in an alternative manner. 
+ derive the compact architecture by pruning redundant paths

## Make Latency Differentiable

Model the latency of a network as a continuous function of the neural network dimensions.

![Making latency differentiable by introducing latency regularization loss](https://images-cdn.shimo.im/4wwTe3kgMO4AJleJ/image.png)