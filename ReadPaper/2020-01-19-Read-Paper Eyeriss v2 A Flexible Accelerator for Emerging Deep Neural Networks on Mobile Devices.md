---
layout: post
title: "[Read Paper] Eyeriss v2: A Flexible Accelerator for Emerging Deep Neural Networks on Mobile Devices"
description: "Reading note of Eyeriss v2: A Flexible Accelerator for Emerging Deep Neural Networks on Mobile Devices"
categories: [ReadPaper]
tags: [EyerissV2, HM-NoC, Eyeriss, DLA]
last_updated: 2020-03-07 12:57:00 GMT+8
excerpt: "Sze Vivienne's Paper. This article contains more details of `Eyeriss V2`."
redirect_from:
  - /2020/01/19/
---

* Kramdown table of contents
{:toc .toc}
# Eyeriss v2: A Flexible Accelerator for Emerging Deep Neural Networks on Mobile Devices

This article contains more details of `Eyeriss V2` than [another Eyeriss V2 paper](../../../../2019/12/29/Read-Paper-Eyeriss-v2-A-Flexible-and-High-Performance-Accelerator-for-Emerging-Deep-Neural-Networks/).

And here is the list of `Eyeriss` series papers:

+ Eyeriss v2: A Flexible Accelerator for Emerging Deep Neural Networks on Mobile Devices
+ [Eyeriss v2: A Flexible and High-Performance Accelerator for Emerging Deep Neural Networks](../../../../2019/12/29/Read-Paper-Eyeriss-v2-A-Flexible-and-High-Performance-Accelerator-for-Emerging-Deep-Neural-Networks/)
+ [Eyeriss: A Spatial Architecture for Energy-Efficient Dataflow for Convolutional Neural Networks](../../../../2019/12/29/Read-Paper-Eyeriss-A-Spatial-Architecture-for-Energy-Efficient-Dataflow-for-Convolutional-Neural-Networks/)
+ [Eyeriss: An Energy-Efficient Reconfigurable Accelerator for Deep Convolutional Neural Networks](../../../../2019/12/29/Read-Paper-Eyeriss-An-Energy-Efficient-Reconfigurable-Accelerator-for-Deep-Convolutional-Neural-Networks/)

## Hierarchical Architecture of Eyeriss

### Top Level

The PEs and GLBs are grouped into clusters to support a flexible on-chip network (NoC) that connects the GLBs to the PEs at low cost.

![Eyeriss v2 top-level architecture](https://images-cdn.shimo.im/GfZZt2o1Omo0Px4X/image.png)



![the components in the architecture](https://images-cdn.shimo.im/rdHBVt49L3Ukg9eb/image.png)

### Hierarchical Mesh Network (HM-NoC)

The HM-NoC can be configured into four different modes depending on the data reuse opportunity and bandwidth requirements.

+ In the high bandwidth mode, each GLB bank or off-chip data I/O can deliver data independently to the PEs in the cluster, which achieves unicast.
+ In the high reuse mode, data from the same source can be routed to all PEs in different clusters, which achieves broadcast.
+ For situations where the data reuse cannot fully utilize the entire PE array with broadcast, different multicast modes, specifically grouped-multicast and interleaved multicast, can be adapted according to the desired multicast patterns.

![Hierarchical mesh network for input activations. This only shows the top 2×2 of the entire 8×2 cluster array](https://images-cdn.shimo.im/2eS389Ug4u84Mujp/image.png)



![Hierarchical mesh network for weights. This only shows the top 2×2 of the entire 8×2 cluster array](https://images-cdn.shimo.im/CWvcp5kSbPMpEQEx/image.png)



![Hierarchical mesh network for psums. This only shows a 2×2 portion of the entire 8×2 cluster array](https://images-cdn.shimo.im/HdYFi0MzVIgkyYm0/image.png)

HM-NoC adapts different modes for different types of layers:

+ Conventional CONV layers: In normal CONV layers, there is plenty of data reuse for both iacts and weights. To keep all 4 PEs busy at the lowest bandwidth requirement, we need 2 iacts and 2 weights from the data source (ignoring the reuse from SPad). In this case, either the HM-NoC for iact or weight has to be configured into the grouped-multicast mode, while the other one configured into the interleaved-multicast mode.
+ Depth-wise (DP) CONV layers: For DP CONV layers, there can be nearly no reuse for iacts due to the lack of output channels. Therefore, we can only exploit the reuse of weights by broadcasting the weights to all PEs while fetching unique iacts for each PE.
+ Fully-connected (FC) layers: Contrary to the DP CONV layers, FC layers usually see little reuse for weights, especially when the batch size is limited. In this case, the modes of iact and weight NoCs are swapped from the previous one: the weights are now unicast to the PEs while the iacts are broadcast to all PEs.

### Eyeriss v2 PE Architecture

To handle these dependencies while still maintaining throughput, the PE is implemented using seven pipeline stages and five SPads:

+ The first two pipeline stages are responsible for fetching non-zero iacts from the SPads.

+ After a non-zero iact is fetched, the next three pipeline stages read the corresponding weights.
+ The final two stages in the pipeline perform the MAC computation on the fetched non-zero iact and weight and then send the updated psum either back to the psum SPad or out of the PE.
+ The iact address SPad stores the address vector of the CSC compressed iacts, which is used to address the reads from the iact data SPad that holds the non-zero data vector as well as the count vector.
+ There is a weight address SPad to address the reads from the weight data SPad for the correct column of weights.
+ There is a psum SPad

![Eyeriss v2 PE Architecture. The address SPad for both iact and weight are used to store address vector in the CSC compressed data, while the data SPad stores the data and count vectors](https://images-cdn.shimo.im/Zj7Aa6YzIis6Fhxs/image.png)

And this is the data format this architecture uses. But I will use a more simple one.

![Example of compressing sparse weights with compressed sparse column (CSC) coding](https://images-cdn.shimo.im/dwf3TL2QC4EdrmXJ/image.png)

| data | row  | col  |
| ---- | ---- | ---- |
| a    | 1    | 0    |
| b    | 2    | 0    |
| c    | 0    | 1    |
| d    | 1    | 1    |
| e    | 3    | 1    |
| f    | 2    | 2    |
| g    | 3    | 4    |
| h    | 1    | 5    |
| i    | 3    | 5    |
| j    | 0    | 7    |
| k    | 1    | 7    |
| l    | 2    | 7    |