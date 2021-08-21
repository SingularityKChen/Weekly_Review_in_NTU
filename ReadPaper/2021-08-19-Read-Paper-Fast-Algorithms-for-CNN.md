---
layout: post
title: "[Read Paper] Fast Algorithms for Convolutional Neural Networks"
description: "WWinograd’ s minimal filtering algorithms compute minimal complexity convolution over small tiles, which makes them fast with small filters and small batch sizes. However, this paper introduces only stride 1."
categories: [ReadPaper]
tags: [CNN, Winograd]
last_updated: 2021-08-19 12:02:00 GMT+8
excerpt: "Winograd’ s minimal filtering algorithms compute minimal complexity convolution over small tiles, which makes them fast with small filters and small batch sizes. However, this paper introduces only stride 1."
redirect_from:
  - /2021/08/19/
---

* Kramdown table of contents
{:toc .toc}
# Fast Algorithms for Convolutional Neural Networks

## Fast Algorithms

It has been known since at least 1980 that the minimal filtering algorithm for computing m outputs with an r-tap FIR filter, which we call F (m, r), requires

```
μ(F(m,r))=m+r−1
```

multiplications. Also, we can nest minimal 1D algorithms F (m, r) and F (n, s) to form minimal 2D algo- rithms for computing m × n outputs with an r × s filter, which we call F (m × n, r × s). These require

```
μ(F (m × n, r × s)) = μ(F (m, r))μ(F (n, s)) = (m + r − 1)(n + s − 1)
```

multiplications. It is interesting to note that in 1D, 2D, and multi- dimensions, the minimal algorithm requires a number of multiplications equal to the number of inputs. In other words, to compute F (m, r) we must access an interval of m + r − 1 data values, and to compute F (m × n, r × s) we must access a tile of (m + r − 1) × (n + s − 1) data values. Therefore the minimal filtering algorithm **requires one multiplication per input**.

![F(2x2,3x3)](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/202108211158643.png)

From equation 7 we can found that we can simply implement those transforming via shift and add.

The number of additions and constant multiplications required by the minimal Winograd transforms increases quadratically with the tile size. Thus for large tiles, the complexity of the transforms will overwhelm any savings in the number of multiplications.
