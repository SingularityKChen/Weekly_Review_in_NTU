---
layout: post
title: "[Glean] ResNet-50 Architecture and # MACs"
description: "ResNet-50 Architecture and # MACs"
categories: [Glean]
tags: [ResNet-50, DLA]
last_updated: 2021-04-03 17:50:00 GMT+8
excerpt: "This posts shows the basic architecture of the ResNet-50 and the number of weights as well as the MAC operations."
redirect_from:
  - /2021/04/03/
---

* Kramdown table of contents
{:toc .toc}
# ResNet-50 Architecture and # MACs

## ResNet-50 Architecture[^1]

![Architectures for ResNet](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20210403172705.png)

From the figure above, ResNet-50 contains 2 separate convolutional layers plus 16 building block where each building block contains three convolutional layers.

## Building Block[^1]

![Residual Learning building block](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20210403171953.png)

The building block in residual learning contains one residual representations and one shortcut connections which skipping one or more layers.

In ResNet-50, the shortcut connections skipping three layers.

## # Weights and # MACs[^2]

![Number of Weights and MACs](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20210403173851.png)

We can conclude that the last three building blocks occupy more weights while their computation operations near the same between different building blocks.

[^1]: K. He, X. Zhang, S. Ren, and J. Sun, “Deep Residual Learning for Image Recognition,” *arXiv:1512.03385 [cs]*, Dec. 2015, Accessed: Dec. 22, 2019. [Online]. Available: http://arxiv.org/abs/1512.03385.

[^2]: 经典神经网络参数的计算【不定期更新】, https://zhuanlan.zhihu.com/p/49842046