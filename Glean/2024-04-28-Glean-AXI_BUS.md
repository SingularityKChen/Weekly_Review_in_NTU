---
layout: post
title: "[Glean] AXI Bus Introduction"
description: "AXI Bus Introduction, including five channels: read address, read data, write address, write data, and write response, and VALID/READY Handshake."
categories: [Glean]
tags: [AXI, AMBA]
last_updated: 2024-08-19 17:06:00 GMT+8
excerpt: "AXI Bus Introduction, including five channels: read address, read data, write address, write data, and write response, and VALID/READY Handshake."
redirect_from:
  - /2024/04/28/
---

* Kramdown table of contents
{:toc .toc}

# AXI Bus

## Introduction[^1]

AXI contains five channels: read address, read data, write address, write data, and write response. Each channel has its own independent protocol.
+ The read address channel (AR) is used to send read requests to the slave
+ The read data channel is (R) used to send read data from the slave to the master
+ The write address channel (AW) is used to send write requests to the slave
+ The write data channel (W) is used to send write data from the master to the slave
+ The write response channel (B) is used to send write responses from the slave to the master

## VALID/READY Handshake[^2]

The AXI protocol uses a valid/ready handshake mechanism to ensure that **both the master and the slave are ready to transfer data.** The master asserts the valid signal to indicate that it has data to transfer, and the slave asserts the ready signal to indicate that it is ready to accept the data. **The transfer of data occurs when both the valid and ready signals are asserted.**

VALID 信号一旦置起就不能置低，直到完成握手（handshake occurs）,至少传输一周期数据。

The VALID signal must remain asserted until the handshake occurs, and at least one data transfer occurs.[^3]

# Reference

[^1]: [深入 AXI4 总线（二）架构](https://zhuanlan.zhihu.com/p/45122977)
[^2]: [深入 AXI4 总线（一）握手机制](https://zhuanlan.zhihu.com/p/44766356)
[^3]: [AXI总线总结](https://www.lzrnote.cn/2021/10/08/axi%E6%80%BB%E7%BA%BF%E6%80%BB%E7%BB%93/)
