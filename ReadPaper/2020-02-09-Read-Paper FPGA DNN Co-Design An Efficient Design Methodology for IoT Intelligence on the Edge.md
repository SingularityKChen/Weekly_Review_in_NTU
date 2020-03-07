---
layout: post
title: "[Read Paper] FPGA/DNN Co-Design: An Efficient Design Methodology for IoT Intelligence on the Edge"
description: "Reading note of FPGA/DNN Co-Design: An Efficient Design Methodology for IoT Intelligence on the Edge"
categories: [ReadPaper]
tags: [CoDesign, DLA]
last_updated: 2020-03-15 11:02:00 GMT+8
excerpt: "Develop DNNs and the corresponding FPGA accelerators simultaneously. DNN designs should be FPGA-architecture driven, and FPGA accelerators should be DNN-aware."
redirect_from:
  - /2020/02/09/
---

* Kramdown table of contents
{:toc .toc}

# FPGA/DNN Co-Design: An Efficient Design Methodology for IoT Intelligence on the Edge

Develop DNNs and the corresponding FPGA accelerators simultaneously. DNN designs should be FPGA-architecture driven, and FPGA accelerators should be DNN-aware.

## Contributions

- simultaneous FPGA/DNN co-design methodology with
  - hardware-oriented DNN model design following **bottom-up approach**;
  - DNN-driven FPGA accelerator design following **top-down approach**.
- For DNN model design, we introduce a DNN template to guide the DNN generation with **predictable performance** and **resource utilization**, which greatly reduces the co-design search space. Based on such template, an automatic DNN model search engine, `Auto-DNN`, is proposed to effectively explore the design space and generate DNN models for desired QoR.
- For FPGA accelerator design, we introduce a fine-grained tile-based pipeline architecture, which supports arbitrary DNNs generated by `Auto-DNN` using a library of highly optimized HLS IPs. Based on such architecture, an automatic HLS generator, `Auto-HLS`, is proposed to directly generate synthesizable C code of the DNN models, to conduct latency/resource estimation and FPGA accelerator generation.
- We demonstrate our co-design approach on an **object detection task** targeting a PYNQ-Z1 embedded FPGA. DNN models are searched and mapped to the board with the state-of-the-art performance regarding accuracy, speed, and power efficiency.

## Some Knowledge About DNN

- DNN design is conducted either manually by machine learning experts or automatically by Neural Architecture Search (NAS) such as recursive neural networks (RNN) and reinforcement learning.
- quantization and model compression are used to reduce DNN model size
- latency-directed resource allocation and fine-grained pipeline architecture are proposed to deliver low latency during DNN inference

## FPGA/DNN Co-Design

### Design Space

- DNN design
  - the number and types of layers
  - the number of input/output channels
  - residual connections
  - concatenations
- FPGA accelerator
  - IP instance categories
  - IP reuse strategies
  - quantization schemes
  - parallel factors
  - data transfer behaviors
  - buffer sizes

For FPGA accelerator, use IP-based design strategy. Each IP supports a basic DNN layer type (e.g. Conv, Pooling), which must be instantiated and configured if the DNN model contains such type of layer.

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200208162333Key%20Variables%20for%20FPGA/DNN%20Co-Design.png" alt="Key Variables for FPGA/DNN Co-Design" style="zoom: 50%;" />

### Co-Design Flow

- Co-Design Step 1: **Building block and DNN modeling**. Capture the **hardware latency** and **resource utilization** of DNN building blocks and hardware IP pool.

- Co-Design Step 2: **Building block selection**. 
  - `Auto-DNN` performs both coarse and fine-grained evaluations of the building blocks regarding three most important features: **latency**, **resource utilization** and **accuracy**. 
  - Based on the evaluation, building blocks on the Pareto curve will be selected for further DNN exploration.
- Co-Design Step 3: **Hardware-aware DNN search and update**. 
  - Given selected building blocks, `Auto-DNN` explores the DNNs under given resource and latency constraints by using stochastic coordinate descent (SCD). 
  - DNNs output from SCD are passed to `Auto-HLS` to get more precise performance and resource results
  - Are fed back to SCD for update. 
  - The generated DNNs that meet performance and resource requirements are output for training and fine-tuning.

### Four Components

![The overall FPGA/DNN co-design flow](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200208163553The%20overall%20FPGA/DNN%20co-design%20flow.png)

- `Bundle-Arch` as a hardware-aware DNN template (green)
- `Tile-Arch` as a low-latency accelerator template (yellow)
- `Auto-DNN` for DNN exploration (blue), works as the primary component and outputs DNN models
- `Auto-HLS ` for FPGA accelerator synthesizable C code generation (pink), outputs the corresponding FPGA implementations of the DNN models