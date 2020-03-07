---
layout: post
title: "[Read Paper] Eyeriss: An Energy-Efficient Reconfigurable Accelerator for Deep Convolutional Neural Networks"
description: "Reading note of Eyeriss: An Energy-Efficient Reconfigurable Accelerator for Deep Convolutional Neural Networks"
categories: [ReadPaper]
tags: [Eyeriss, DLA, DNN]
last_updated: 2020-03-07 13:37:00 GMT+8
excerpt: "Sze Vivienne's Paper. This article introduces the way to reduce the cost of data movement by exploiting data reuse in a multilevel memory hierarchy. Also, techniques such as compression and data-adaptive processing are introduced to save both memory bandwidth and processing power."
redirect_from:
  - /2019/12/29/
---

* Kramdown table of contents
{:toc .toc}

# Eyeriss: An Energy-Efficient Reconfigurable Accelerator for Deep Convolutional Neural Networks

Compared to the [Eyeriss v2](../Read-Paper-Eyeriss-v2-A-Flexible-and-High-Performance-Accelerator-for-Emerging-Deep-Neural-Networks/) and [Spatial Architecture](../Read-Paper-Eyeriss-A-Spatial-Architecture-for-Energy-Efficient-Dataflow-for-Convolutional-Neural-Networks/), this article provides a more detailed explanation on the system architecture of the NoC communication as well as mapping of the PE sets. Also, it introduces an RLC compressed form to reduce the data size as related components in PE architecture.

Plus, this article mentions CNN biases, but there is nothing related to the biases in the architecture it describes.

---

This article introduces the way to reduce the cost of data movement by exploiting data reuse in a multilevel memory hierarchy. Also, techniques such as compression and data-adaptive processing are introduced to save both memory bandwidth and processing power.

## Main Features of Eyeriss

+ A four-level memory hierarchy. Data movement can exploit low-cost levels:
  + the PE scratch pads
    + the inter-PE communication
    + the large on-chip global buffer (GLB)
    + the off-chip DRAM.
+ A CNN dataflow, called Row Stationary (RS), that reconfigures the spatial architecture to map the computation of a given CNN shape and optimize for the best energy efficiency.
+ A network-on-chip (NoC) architecture that uses both multicast and point-to-point single-cycle data delivery to support the RS dataflow.
+ Run-length compression (RLC) and PE data gating that exploits the statistics of zero data in CNNs to further improve energy efficiency.

This is the architecture of Eyeriss system:

![Eyeriss system architecture](https://images-cdn.shimo.im/KUn0Ny1sa6wMuMuy/image.png)

## System Control and Configuration

The accelerator has two levels of control hierarchy. 

+ The top-level control coordinates:
  + traffic between the off-chip DRAM and the GLB through the asynchronous interface;
    + traffic between the GLB and the PE array through the NoC;
    + operation of the RLC CODEC and ReLU module. 
+ The lower-level control consists of control logic in each PE, which runs independently of each other.

The size:

+ scratch pad capacity depends only on the filter row size (S)
  but not the input feature map row size (W), and is equal to:
  	+ S for a row of filter weights; 
  	+ S for a sliding window of input feature map values;
  	+ 1 for the partial sum accumulation.

+ the height and the width of the PE set are equal to the number of filter rows (R) and ofmap rows (E), respectively.

![Mapping of the PE sets on the spatial array of 168 PEs for the CONV layers in AlexNet](https://images-cdn.shimo.im/JlEPRdIC1rU50uoT/image.png)

## Exploit Data Statistics

data statistics of CNN is explored to:

+ reduce DRAM accesses using compression, which is the most energy-consuming data movement per access, on top of the optimized dataflow;
+ skip the unnecessary computations to save processing power

### RLC

RLC is used in Eyeriss to exploit the zeros in fmaps and save DRAM bandwidth.

![an example of RLC encoding](https://uploader.shimo.im/f/Ir9k6AuBWP0NWomF.png!thumbnail)

### NoC

Requirements of the NoC architecture:

+ support the data delivery patterns used in the RS dataflow; should be taken care of:
  + different convolution strides (U) result in the ifmap delivery, skipping certain rows in the array;
    + a set is divided into segments that are mapped onto different parts of the PE array;
    + multiple sets are mapped onto the array simultaneously and different data is required for each set
+ leverage the data reuse achieved by the RS dataflow to further improve energy efficiency;
+ provide enough bandwidth for data delivery to support the highly parallel processing in the PE array.

Three different types of networks:

+ Global Input Network: is optimized for a single-cycle multicast from the GLB to a group of PEs that receive the same filter weight, input feature map value, or partial sum. The challenge is that the group of destination PEs varies across layers due to the differences in data type, convolution stride, and mapping.
+ Global Output Network: The GON is used to read the psums generated by a processing pass from the PE array back to the GLB.
+ Local Network: pass the psums from two consecutive rows of the same column

![Architecture of the GIN](https://images-cdn.shimo.im/RrYcTIHJCF4DOYXc/image.png)

### Processing Element and Data Gating

The data path is pipelined into three stages: one stage for scratch pad access, and the remaining two for computation.

An extra 12-b Zero Buffer is used to record the position of zeros in the input feature map scratch pad. If a zero input feature map value is detected from the zero buffer, the gating logic will disable the read of the filter scratch pad and prevent the MAC data path from switching.

![PE architecture](https://images-cdn.shimo.im/MPBbaUOFLNEcqibp/image.png)