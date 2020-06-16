---
layout: post
title: "[Glean] 3D Convolution: kernel will traverse in 3-D"
description: "This post introduces the 3D convolution."
categories: [Glean]
tags: [3DConv]
last_updated: 2020-06-12 23:24:00 GMT+8
excerpt: "This post introduces the 3D convolution."
redirect_from:
  - /2020/06/12/
---

* Kramdown table of contents
{:toc .toc}
# 3D Convolution: kernel will traverse in 3-D

A 3D Convolution can be used to find patterns across 3 spatial dimensions; i.e. depth, height and width. One effective use of 3D Convolutions is object segmentation in 3D medical imaging. Since a 3D model is constructed from the medical image slices, this is a natural fit. And action detection in video is another popular research area, where multiple image frames are concatenated across a temporal dimension to give a 3D spatial input, and patterns are found across frames too.

Spatial dimensions are distinct from the channels dimension. Although color images have 3 channels, there are still only 2 spatial  dimensions (of height and width), making 2D Convolutions more  appropriate in this case. And RGB videos will still have 3 spatial  dimensions (of time, height and width), making 3D Convolutions ideal.

![](https://miro.medium.com/max/1400/1*ROD5gvIR8Octbo0uk6RzzA.gif)

![](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6720945/bin/sensors-19-03579-g005.jpg)