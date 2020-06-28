---
layout: post
title: "[Workshop] tinyML Talks: Saving 95% of Your Edge Power with Sparsity"
description: "It will explain these types of sparsity (time, space, connectivity, activation) in terms of edge processes, and how they affect computation on a practical level."
categories: [Workshop]
tags: [tinyML, Sparsity]
last_updated: 2020-06-28 10:50:00 GMT+8
excerpt: "It will explain these types of sparsity (time, space, connectivity, activation) in terms of edge processes, and how they affect computation on a practical level."
redirect_from:
  - /2020/06/28/
---

* Kramdown table of contents
{:toc .toc}
# tinyML Talks - Jon Tapson: Saving 95% of Your Edge Power with Sparsity to Enable tinyML[^1]

## Characteristics of Edge Data Streams

### The data rate is much higher than the real information rate

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200628100203.png" style="zoom:50%;" />

### Sensors or systems are idle for long periods or large duty cycles (even during input)

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200628100247.png" style="zoom:50%;" />

### Most inputs have high levels of redundancy

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200628100523.png" alt="high levels of redundancy" style="zoom:50%;" />

## Sparse Compute

+ SPARSITY in  SPACE: “Curse of dimensionality” =–as data dimension grows, the proportion of null data points grows exponentially
+ SPARSITY in  TIME: Real world signals have sparse changes
+ SPARSITY in  CONNECTIVITY: Compute only graph edges **with significant weight** (exploit “small world” connectivity)
+ SPARSITY in  ACTIVATION: Less than 40% of neurons may be activated by **an upstream change**

### Sparsity in Space

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200628103050.png" alt="SPARSITY in  SPACE" style="zoom:50%;" />

### Event Based Convolutions

Single events in an input layer fan out (typically 1:9 per feature layer)  Only the affected pixels in the convolutional layer need be computed.

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200628104633.png" style="zoom:50%;" />

The events then fan in typically 4:1 in pooling layers. Additionally, events are only 25% likely to change the pixel state (typ. 2x2 max pooling)

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200628104754.png" style="zoom:50%;" />

When events are localized, these numbers improve dramatically because the convolutional patches overlap.

Locality is preserved downstream.

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200628104946.png" alt="events are localized" style="zoom:50%;" />

### Exploiting Sparsity in Compute Architectures

Each time, just the difference of the previous frame should be calculated, i.e., the corresponding neurons will be activated by the changes.

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200628103345.png" style="zoom:50%;" />

[^1]: tinyML, [YouTube](https://www.youtube.com/watch?v=zso5qsduGhQ)