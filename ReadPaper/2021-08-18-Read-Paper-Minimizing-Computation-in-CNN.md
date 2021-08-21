---
layout: post
title: "[Read Paper] Minimizing Computation in Convolutional Neural Networks"
description: "Strassen algorithm can compute 2x2 Matrix Mult using only 7 multiplications."
categories: [ReadPaper]
tags: [CNN, Strassen]
last_updated: 2021-08-18 11:57:00 GMT+8
excerpt: "Strassen algorithm can compute 2x2 Matrix Mult using only 7 multiplications."
redirect_from:
  - /2021/08/18/
---

* Kramdown table of contents
{:toc .toc}
# Minimizing Computation in Convolutional Neural Networks

## Properties of CNN Computation

![Properties of CNN Computation](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/202108211042838.png)

## Computation Optimization

In this section we show how to extend the Strassen algorithm from Normal MM to reduce the computational workload of our Convolutional MM. 

![Computation Optimization](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/202108211043058.png)

We can see that each recursion will reduce the number of multiplications by 1/8, but will incur 18 additions on the submatrices. The overhead of 18 additions could completely eliminate the benefits brought by the multiplication savings in normal MMs.