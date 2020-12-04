---
layout: post
title: "[Glean] Design Compiler ddc file"
description: "In general, it is binary file which contains both verilog gate level description and design constrains."
categories: [Glean]
tags: [Design Compiler, ddc]
last_updated: 2020-11-24 10:23:00 GMT+8
excerpt: "In general, it is binary file which contains both verilog gate level description and design constrains."
redirect_from:
  - /2020/11/24/
---

* Kramdown table of contents
{:toc .toc}
# Design Compiler ddc file

In general, it is binary file which contains both verilog gate level description and design constrains.

`.ddc` consists of the same information as a `.db` file. `ddc` is a Synopsys  encrypted form of your design which can be read by the tools such as  Design compiler, IC compiler and prime time. It consists of the  netlist(list of components and nets) information of your design along  with the constraints which you have specified for implementing the design.