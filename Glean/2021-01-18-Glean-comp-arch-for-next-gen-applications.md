---
layout: post
title: "[Glean] Computer Architectures for Next Generation Applications"
description: "This post is mainly translated from one zhihu answer by Bao Yungang. It introduces three laws: Moore_s law, Makimoto_s wave, Bell_s law and design methods and optimizations for performance and power as well as fragmented requirements in AIoT aging. These methods including reducing data movements, reducing data precious, improve parallelism and agile hardware development."
categories: [Glean]
tags: [Moore, Makimoto, Bell, DSA, Parallelism, Agile]
last_updated: 2021-01-18 17:07:00 GMT+8
excerpt: "This post is mainly translated from one zhihu answer by Bao Yungang. It introduces three laws: Moore_s law, Makimoto_s wave, Bell_s law and design methods and optimizations for performance and power as well as fragmented requirements in AIoT aging. These methods including reducing data movements, reducing data precious, improve parallelism and agile hardware development."
redirect_from:
  - /2021/01/18/
---

* Kramdown table of contents
{:toc .toc}
# Computer Architectures for Next Generation Applications[^1]

## Three Laws: Moore's law, Makimoto's wave, Bell's law

### Moore's law

Moore's law is not end, but slower.

![a new golden age for computer architecture](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20210118154938.png)

Not all the transistors are used wisely[^2]. They can speed up the performance of one Python program by $62,806\times$ via fully utilizing CPU.

From the software engineers' aspect, this huge performance gap means that most of the engineers can not full utilize CPU.

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20210118155549.png)

One possible method can be utilizing **domain specific architectures**. This is one method to implement the algorithms into hardware directly instead of using software programs, which can be orders of magnitude speeds up.

### Makimoto's wave

In the early 1990s, Dr Tsugio Makimoto made an observation while working as Sony’s chief technology officer. He had observed that electronics cycled **between custom solutions and programmable** ones approximately every ten years.[^3]

![Makimoto’s Wave](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20210118160644_Makimoto_Wave.webp)

Things behind this law is the trade off between power and performance. As for CPU, its the balance between general purpose architecture and DSA, which represent a higher development efficient and a better performance respectively.

### Bell's law

Roughly every decade **a new, lower priced computer class** forms based  on a new programming platform, network, and interface resulting in new usage and the establishment of **a new industry**.

Coming into the AIoT aging, the demanding of processors will burst while the requirements are also fragmented. This can be a challenge to current design methodologies and industry to fulfill fragmented requirements.[^1]

## Design Methods and Optimzations for DSA

### Performance and power

#### Reduce data movements

+ custom instructions can reduce the number of instructions and optimize some operations
+ utilize cache. replacement, prefetch, compress, schedule.

#### Reduce data precious

Mixed precious data computation.

May use Posit data format to mix the data precious.

#### Improve parallelism

Instruction Level Parallelism(ILP), Memory Level Parallelism(MLP)

Superscalar, Multithreading, SIMD, STMD, Systolic Array

### Fragmented requirements

+ [agile design](https://singularitykchen.github.io/blog/2020/04/26/2020-04-20-26-weekly-review/#heading-agile-hardware-development)

### History of CPU

![](https://pic4.zhimg.com/80/v2-9bdeb0f0ac3062aec5ab89c7f6b37b8d_720w.jpg?source=1940ef5c)

The state changing of CPU is driven by the software program. Therefore, the optimization in the CPU level is finding the common behaviors in the programs and accelerate them, which requires the knowledge in software programming, operation system, compiler and computer architecture.

[^1]: "多核之后，CPU的发展方向是什么 - 知乎", https://www.zhihu.com/question/20809971/answer/1678502542 (Accessed: Jan. 18, 2021.).

[^2]: C. E. Leiserson et al., “There’s plenty of room at the Top: What will drive computer performance after Moore’s law?,” Science, vol. 368, no. 6495, Jun. 2020, doi: 10.1126/science.aam9744.

[^3]: “Makimoto’s Wave,” Semiconductor Engineering. https://semiengineering.com/knowledge_centers/standards-laws/laws/makimotos-wave/ (accessed Jan. 18, 2021).
