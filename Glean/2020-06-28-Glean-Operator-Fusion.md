---
layout: post
title: "[Glean] Operator Fusion"
description: "There are many opportunities, where fused operators—in terms of fused chains of basic operators—can significantly improve performance."
categories: [Glean]
tags: [OperatorFusion]
last_updated: 2020-06-29 09:20:00 GMT+8
excerpt: "There are many opportunities, where fused operators—in terms of fused chains of basic operators—can significantly improve performance."
redirect_from:
  - /2020/06/28/
---

* Kramdown table of contents
{:toc .toc}
# Operator Fusion[^1]

There are many opportunities, where fused operators—in terms of fused chains of basic operators—can significantly improve performance. 

First, fusion allows eliminating the **unnecessary materialization of intermediates**, whose allocation and write is often a bottle-neck.

Second, fusion can eliminate **unnecessary scans of inputs** by exploiting temporal cell or row locality.

Third, multiple aggregates with shared inputs ever-age similar opportunities for DAGs (directed acyclic graphs) of multiple aggregates over common subexpressions (CSEs).

Fourth, “sparse drivers” allow **sparsity exploitation** across entire chains of operations.

[^1]: Boehm, Matthias, et al. "On optimizing operator fusion plans for large-scale machine learning in systemml." *arXiv preprint arXiv:1801.00829* (2018).