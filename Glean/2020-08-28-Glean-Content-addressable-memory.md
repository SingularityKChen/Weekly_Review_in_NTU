---
layout: post
title: "[Glean] Content-addressable memory"
description: "Content-addressable memories (CAMs) are hardware search engines that are much faster than algorithmic approaches for search-intensive applications. CAMs are composed of conventional semiconductor memory (usually SRAM) with added comparison circuitry that enable a search operation to complete in a single clock cycle. The two most common search-intensive tasks that use CAMs are packet forwarding and packet classification in Internet routers. I introduce CAM architecture and circuits by first describing the application of address lookup in Internet routers. Then we describe how to implement this lookup function with CAM."
categories: [CAM, Cache]
tags: [CodeBloat]
last_updated: 2020-08-28 21:57:00 GMT+8
excerpt: "Content-addressable memories (CAMs) are hardware search engines that are much faster than algorithmic approaches for search-intensive applications. CAMs are composed of conventional semiconductor memory (usually SRAM) with added comparison circuitry that enable a search operation to complete in a single clock cycle. The two most common search-intensive tasks that use CAMs are packet forwarding and packet classification in Internet routers. I introduce CAM architecture and circuits by first describing the application of address lookup in Internet routers. Then we describe how to implement this lookup function with CAM."
redirect_from:
  - /2020/08/28/
---

* Kramdown table of contents
{:toc .toc}
# Content-addressable memory[^1]

**Content-addressable memory** (**CAM**) is a special type of computer memory used in certain very-high-speed searching applications. It is also known as **associative memory** or **associative storage** and compares input search data (tag) against a table of stored data,  and *returns the address* of matching data (or in the case of associative memory, the *matching data*).

CAM is frequently used in networking devices where it speeds forwarding information base and routing table operations.

![](https://d33wubrfki0l68.cloudfront.net/e96e47cd75fe4a21c98299653088e7ec87e95d2a/0584c/cam/images/camram.png)

CAM can be seen as the hardware implementation of associative array.

To achieve a different balance between speed, memory size and cost,   some implementations emulate the function of CAM by using standard tree  search or hashing designs in hardware, using hardware tricks like  replication or pipelining to speed up effective performance. These  designs are often used in routers.

[^1]: Content-addressable memory. https://en.wikipedia.org/wiki/Content-addressable_memory

