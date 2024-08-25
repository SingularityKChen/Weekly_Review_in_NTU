---
layout: post
title: "[Glean] Outstanding Transactions"
description: "This post explains the concept of outstanding transactions."
categories: [Glean]
tags: [Outstanding]
last_updated: 2024-08-19 17:06:00 GMT+8
excerpt: "This post explains the concept of outstanding transactions."
redirect_from:
  - /2024/02/01/
---

* Kramdown table of contents
{:toc .toc}

# Outstanding

## 定义

"Outstanding" 事务指的是已经启动但尚未完成的事务。

在一个系统中，可以同时有多个 outstanding 事务，特别是在支持并发操作的系统中。

Outstanding 事务的数量直接影响系统的吞吐量和性能，因为它们代表了系统正在处理的工作量。例如，在处理器访问内存或 IO 设备时，可以同时发出多个读写请求，这些尚未完成的请求就是 outstanding 事务。

## 适用情况

在设计高性能计算系统和数据通信协议时，管理 outstanding 事务的数量是优化性能的关键。系统设计需要平衡 outstanding 事务的数量，以充分利用资源，同时避免产生拥塞或超出系统处理能力。
