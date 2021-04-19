---
layout: post
title: "[Glean] What Is Memory-Hard"
description: "What Is Memory-Hard"
categories: [Glean]
tags: [Linzhi, Memory Hard, E1400]
last_updated: 2021-04-19 16:38:00 GMT+8
excerpt: "In cryptography, a memory-hard function (MHF) is a function that costs significant amount of memory to evaluate. I also show the solution from Linzhi."
redirect_from:
  - /2021/04/19/
---

* Kramdown table of contents
{:toc .toc}
# What Is Memory-Hard?

## Wikipedia[^1]

In cryptography, a memory-hard function (MHF) is a function that <u>costs significant amount of memory to evaluate</u>. It is different from a memory-bound function; the latter incurs cost by slowing down computation through memory latency. MHFs find their use as a form of proof of work (PoW).

## Linzhi[^2]

![](https://miro.medium.com/max/700/1*A9XGG5xJpgH7CsX84eReBA.png)

+ Read-many, write-few
+ Memory size chosen to fit inside a CPU L2/L3 cache (can not fit into L1)
+ Lack of information about chip design and economics.

Ethash designers learned from Scrypt, and in 2014 created a two-stage memory structure that is growing over time. The first stage is called the pseudorandom cache, and the second stage called the DAG.

The ratio between pseudorandom cache and DAG was set to 1:64.

The first-stage memory (pseudorandom cache) functions as a deterrent to memory down-sampling. We believe size and growth of it are secondary as long as it’s not smaller than 1 MB. The second-stage memory (DAG) functions as a deterrent to single-die solutions.

Ethash’s two-stage memory system works well to mitigate the use of logic to recalculate data (to replace actual reads from memory), but it now costs more to verify a block.

# Methods from Linzhi[^3]

Linzhi solves this problem by dividing the DAG memories into 72 mixers decentralised memory in several chips.

Each mixer stores part of the DGA and also contains the logic unit to compute the next DAG address.

I think it kinds of in-memory-computing.

![Linzhi E1400](https://miro.medium.com/max/4954/1*VlZWxSFBj7bkkyBfU6F8kg.png)

![Hashing with E1400](https://miro.medium.com/max/5384/1*KGRkYu1IAPeS2ebEXtACbw.png)

[^2]: What Is Memory-Hard? https://medium.com/@Linzhi/what-is-memory-hard-45a363b59dfe
[^1]: https://en.wikipedia.org/wiki/Memory-hard_function
[^3]: Linzhi E1400 — Architecture Overview, https://medium.com/@Linzhi/linzhi-e1400-architecture-overview-6fed5a19ef70