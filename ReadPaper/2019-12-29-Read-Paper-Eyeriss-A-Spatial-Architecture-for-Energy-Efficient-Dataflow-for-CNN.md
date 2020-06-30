---
layout: post
title: "[Read Paper] Eyeriss: A Spatial Architecture for Energy-Efficient Dataflow for Convolutional Neural Networks"
description: "Reading note of Eyeriss: A Spatial Architecture for Energy-Efficient Dataflow for Convolutional Neural Networks"
categories: [ReadPaper]
tags: [Eyeriss, DLA, DNN]
last_updated: 2020-03-07 13:21:00 GMT+8
excerpt: "Sze Vivienne's Paper. This article proposed RS dataflow which can adapt to different CNN shape configurations and reduces all types of data movement through maximally utilizing the processing engine (PE) local storage, direct inter-PE communication and spatial parallelism."
redirect_from:
  - /2019/12/29/
---

* Kramdown table of contents
{:toc .toc}

# Eyeriss: A Spatial Architecture for Energy-Efficient Dataflow for Convolutional Neural Networks

Compared to the [Eyeriss v2](../Read-Paper-Eyeriss-v2-A-Flexible-and-High-Performance-Accelerator-for-Emerging-DNN/), this article provides a more detailed explanation of *Row Stationary*, a baseline storage area for a given number of PEs and the energy cost estimation for RS reuse pattern.

---

This article proposed RS dataflow which can adapt to different CNN shape configurations and reduces all types of data movement through maximally utilizing the processing engine (PE) local storage, direct inter-PE communication and spatial parallelism.

Also, an analysis framework that compares energy cost under the same hardware area and processing parallelism constraints.

## Framework for Energy Efficiency Analysis

### Storage Area

The baseline storage area for a given number of PEs is calculated as

```
#PE×Area(512B RF)+Area((#PE×512B) global buffer).
```

![](https://images-cdn.shimo.im/H6Ir275vy2cj9SXP/image.png)

### Input Data Access Energy Cost

Input data access energy cost estimation:

```
a×EC(DRAM)+ab×EC(global buffer)+abc×EC(array)+abcd×EC(RF)
```

![Example of input data](https://images-cdn.shimo.im/4HIcPqN7ZJYtzTZu/image.png)

### Partial Sum Accumulation Energy Cost

Partial Sum accumulation energy cost estimation:

```
(2a−1)×EC(DRAM)+2a(b−1)×EC(global buffer)+ab(c−1)×EC(array)+2abc(d−1)×EC(RF)
```

![Example of Psum accumulation](https://images-cdn.shimo.im/YpjPqAj7BIkUqJOS/image.png)

where the `EC(*)` is shown as follows:

![NORMALIZED ENERGY COST RELATIVE TO A MAC OPERATION EXTRACTED FROM A COMMERCIAL 65NM PROCESS](https://images-cdn.shimo.im/QQxUYwRr9kEkrpyE/image.png)