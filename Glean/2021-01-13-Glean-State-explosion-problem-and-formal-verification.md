---
layout: post
title: "[Glean] State Explosion Problem and Formal Verification"
description: ""
categories: [Glean]
tags: [verification, formal]
last_updated: 2021-01-13 10:38:00 GMT+8
excerpt: ""
redirect_from:
  - /2021/01/13/
---

* Kramdown table of contents
{:toc .toc}
# State Explosion Problem and Formal Verification[^1]

In a n-bit counter, the number of states of the counter is exponential in the number of bits, i.e., $2^n$. In model checking we refer to this problem as the state explosion problem. All model checkers suffer from it.

**Solutions**:

1. One of the first major advances was symbolic model checking with **binary decision diagrams** (BDDs). In this approach, **a set of states is represented by a BDD instead of by listing each state individually**. The BDD representation is often exponentially smaller in practice. Model checking with BDDs is performed using a fixed point algorithm. 
2. Another major advance is the **partial order reduction**, which e**xploits the independence of actions** in a system with asynchronous composition of processes. 
3. A third major advance is **counterexample-guided abstraction refinement**, which adaptively tries to find **an appropriate level refinement**, precise enough to verify the property of interest yet not burdened with irrelevant detail that slows down verification. 
4. Finally, **bounded model checking** exploits fast Boolean satisfiability (SAT) solvers to search for counterexamples of bounded length.

[^1]: E. M. Clarke, W. Klieber, M. Nováček, and P. Zuliani, “Model Checking and the State Explosion Problem,” in *Tools for Practical Software Verification*, Sep. 2011, pp. 1–30, doi: [10.1007/978-3-642-35746-6_1](https://doi.org/10.1007/978-3-642-35746-6_1). ↩