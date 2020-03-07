---
layout: post
title: "[Read Paper] Learning to Design Circuits"
description: "Reading note of Learning to Design Circuits"
categories: [ReadPaper]
tags: [ML, NAS, L2DC]
last_updated: 2020-03-07 13:02:00 GMT+8
excerpt: "Han Song's Paper: Learning to Design Circuits. Using ML to design analogue circuits."
redirect_from:
  - /2020/01/12/
---

* Kramdown table of contents
{:toc .toc}

# Learning to Design Circuits

Two difficult for searching for parameters that satisfy circuit specifications due to the low availability of training data:

+ Circuit simulation is slow, thus generating large-scale dataset is time-consuming
+ Most circuit designs are propitiatory IPs within individual IC companies, making it expensive to collect large-scale datasets

Constrains for L2DC(Learning to Design Circuits): 

+ meet hard-constraints (eg. gain, bandwidth)

+ optimize good-to-have targets (eg. area, power)

![Learning to Design Circuits (L2DC) Method Overview](https://images-cdn.shimo.im/DygvZ9uF87sLxhUy/image.png)

Steps:

+ leverages reinforcement learning (RL) to generate circuits data by itself and learns from the data to search for best parameters.

+ produces an action (a set of parameters) to the circuit simulator environment, and then receives a reward as a function of gain, bandwidth, power, area, etc.
+ The reward is defined to optimize the desired Figures of Merits (FOM) composed of several performance metrics.
+ By maximizing the reward, RL agent can optimize the circuit parameters.

![use sequence to sequence model to generate circuit parameters](https://images-cdn.shimo.im/AY5WaAFy94ct6a5z/image.png)