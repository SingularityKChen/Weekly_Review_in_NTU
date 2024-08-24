---
layout: post
title: "[Glean] Precision Format"
description: "Precision formats of floating-point and integer."
categories: [Glean]
tags: [data precesion, TensorFloat32, TF32, BrainFloat16, BF16, FloatingPoint, FP32, FP16, FP8
, Integer, INT16, INT8, SINT16, UINT16, SINT8, UINT8]
last_updated: 2024-08-24 8:42:00 GMT+8
excerpt: "Precision formats of floating-point and integer."
redirect_from:
  - /2024/02/21/
---

* Kramdown table of contents
{:toc .toc}

# Precision Format

## Overview

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/202408240850557_data_precision.png" alt="precision_formats" style="background-color:white; margin-left: auto; margin-right: auto; width: 90%;"/>

- Floating-Point Formats
  - FP32 (32 bit)
  - TF32 (19 bit)
  - BF16 (16 bit)
  - FP16 (16 bit)
  - FP8 (8 bit)
    - FP8 E5M2
    - FP8 E4M3
- Integer Formats
  - INT16 (16 bit)
    - SINT16
    - UINT16
  - INT8 (8 bit)
    - SINT8
    - UINT8

## Floating-Point Formats

### FP32 (32 bit)

Floating-Point 32-bit

- 1 bit sign
- 8 bit exponent
- 23 bit mantissa

### TF32 (19 bit)

TensorFloat 19-bit

<mark>TF32 is compatible with both BF16 and FP16</mark>, and is more accurate than BF16.

- 1 bit sign
- 8 bit exponent
- 10 bit mantissa

### BF16 (16 bit)

BrainFloat 16-bit

- 1 bit sign
- 8 bit exponent
- 7 bit mantissa

### FP16 (16 bit)

Floating-Point 16-bit

- 1 bit sign
- 5 bit exponent
- 10 bit mantissa

### FP8 (8 bit)

Floating-Point 8-bit

#### FP8 E5M2

- 1 bit sign
- 5 bit exponent
- 2 bit mantissa

#### FP8 E4M3

- 1 bit sign
- 4 bit exponent
- 3 bit mantissa

## Integer Formats

### INT16 (16 bit)

#### SINT16

Signed Integer 16-bit

- 1 bit sign
- 15 bit mantissa

#### UINT16

Unsigned Integer 16-bit

- 16 bit mantissa

### INT8 (8 bit)

#### SINT8

Signed Integer 8-bit

- 1 bit sign
- 7 bit mantissa

#### UINT8

Unsigned Integer 8-bit

- 8 bit mantissa

# References

- [What is the TensorFloat-32 Precision Format? | NVIDIA Blog](https://blogs.nvidia.com/blog/tensorfloat-32-precision-format/)
