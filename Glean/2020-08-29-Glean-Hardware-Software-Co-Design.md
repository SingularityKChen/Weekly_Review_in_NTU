---
layout: post
title: "[Glean] Hardware/Software Codesign"
description: "As the name implies, Hardware/Software Codesign (HSCD) denotes design methodologies for electronic systems that exploit the trade-offs and the synergy of Hardware (HW) and Software (SW)."
categories: [CAM, Cache]
tags: [Codesign]
last_updated: 2020-08-29 20:13:00 GMT+8
excerpt: "As the name implies, Hardware/Software Codesign (HSCD) denotes design methodologies for electronic systems that exploit the trade-offs and the synergy of Hardware (HW) and Software (SW)."
redirect_from:
  - /2020/08/29/
---

* Kramdown table of contents
{:toc .toc}
# Hardware/Software Codesign[^1]

As the name implies, Hardware/Software Codesign (HSCD) denotes design methodologies for electronic systems that exploit the trade-offs and the synergy of Hardware (HW) and Software (SW). 

Typically, the application functionality is partitioned into software components that are running on the processor cores and hardware components that are used to accelerate some parts of the application or to provide interfaces to the environment. 

In the traditional design practice for such systems, software is usually designed after the hardware architecture is fixed. After this HW/SW partitioning for the determined hardware architecture, two groups of engineers work independently to design and implement the hardware components and software components.

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200829200523.png" alt="A traditional design flow of an electronic system" style="zoom:67%;" />

As the system complexity increases, the above traditional design flow faces several challenges. It is evident that the critical path in the design process becomes prohibitively long and costly in case multiple iterations of the sequential flow are performed from HW and SW development, software porting, and testing.

HW/SW codesign shortens the critical path of the design loop by estimating the performance fast and accurately for a given hardware platform and HW/SW partitioning decision without hardware prototyping.

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200829200734.png" alt="Design flow based on HW/SW codesign" style="zoom:67%;" />

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200829200815.png" alt="Comparison of (a) a classical design flow and (b) an ESL design flow starting from an executable specification and allowing for concurrent development of hardware and software after an initial delay for specification and design space exploration." style="zoom:67%;" />

[^1]: Handbook of Hardware/Software Codesign

