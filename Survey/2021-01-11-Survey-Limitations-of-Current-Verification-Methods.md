---
layout: post
title: "[Survey] Limitations of Current Verification Methods"
description: "This post introduces the limitations of current verification methods, including formal verification, constrained random verification (CRV) and hardware-software co-verification using virtual platform with hardware emulation and acceleration."
categories: [Survey]
tags: [verification, formal, CRV, co-verification]
last_updated: 2021-01-11 14:26:00 GMT+8
excerpt: "This post introduces the limitations of current verification methods, including formal verification, constrained random verification (CRV) and hardware-software co-verification using virtual platform with hardware emulation and acceleration."
redirect_from:
  - /2021/01/11/
---

* Kramdown table of contents
{:toc .toc}
# Limitations of Current Verification Methods

## Formal Verification

### Pros and cons of formal verification[^4]

Model checking is a very powerful framework for verifying specifications of finite state systems. One of the main advantages of model checking is that it is **fully automated**. No expert is required in order to check whether a given finite-state model conforms to a given set of system specifications. Model checking also works with **partial specifications**, which are often troublesome for techniques based on theorem proving. When a property specification does not hold, a model checker can provide a counterexample (an initial state and a set of transitions) that reflects an actual execution leading to an error state. This is the reason why tools based on model checking are very popular for debugging.

One aspect that can be viewed as negative is that model checkers **do not provide correctness proofs**. Another negative aspect is that **model-checking techniques can be directly applied only to finite-state systems**. An infinite-state system can by abstracted into a finite model; however, this leads to a loss of precision. Perhaps the most important issue in model checking is **the state-explosion problem**. It is apparent from the complexity of the CTL model checking algorithm that its practical usefulness critically depends on the size of the state space. If the number of states is too large, then the complexity of the verification procedure may render the technique unusable.

### Missed corner cases[^1]

With the recent trend of parallel processing units, raises the possibility of missed corner cases
that simulation technologies have trouble analyzing. Although formal verification is well suited for finding corner case design bugs, current formal verification technologies are only powerful enough to verify small designs. It is still unknown how to apply formal verification consistently to **larger designs or find corner cases that result from the interaction of multiple modules**. Although there have been successes in using formal verification in verifying large high level designs, the verification methodology successes are time consuming and specific to a particular design.

### The gap between an abstract model and a concrete model[^1]

The other usage of formal verification is for quick verification of protocol ideas before the design process. However, in these verification efforts, it is not clear how to tie the verified protocol models with the real implementation to ensure that the design actually implement the verified model.

Bridging the gap between an abstract model and a concrete model is a problem recognized by the formal verification community even before ESL research has came onto the scene [23]. This is because **formal verification requires abstract models in order to make verification possible**, and it is suggested that the correspondence between the abstract models and implementation can be verified through refinement techniques. However, **all these techniques only work well for layers that are close to each other in terms of abstraction level**, and there is still no practical way to link formally solvable abstract model to the RTL implementation at this time.

### State explosion problem[^4]

In a n-bit counter, the number of states of the counter is exponential in the number of bits, i.e., $2^n$. In model checking we refer to this problem as the state explosion problem. All model checkers suffer from it.

**Solutions**:

1. One of the first major advances was symbolic model checking with **binary decision diagrams** (BDDs). In this approach, **a set of states is represented by a BDD instead of by listing each state individually**. The BDD representation is often exponentially smaller in practice. Model checking with BDDs is performed using a fixed point algorithm. 
2. Another major advance is the **partial order reduction**, which e**xploits the independence of actions** in a system with asynchronous composition of processes. 
3. A third major advance is **counterexample-guided abstraction refinement**, which adaptively tries to find **an appropriate level refinement**, precise enough to verify the property of interest yet not burdened with irrelevant detail that slows down verification. 
4. Finally, **bounded model checking** exploits fast Boolean satisfiability (SAT) solvers to search for counterexamples of bounded length.

## Constrained Random Verification

### Slow simulation speed and resources consumption with large design

Simulation can be very slow if the design is large. Also, as the testcases are randomly generated, there can be redundant testcases, which waste simulation time and computational resources.

**Solutions**:[^5]

1. Higher-level abstractions simulate much faster than pure RTL testbench which is modeled at signal level. Use transaction level testbench (e.g., UVM, TLM)
2. Deploy well-thought-out hardware acceleration.

### Difficult to reuse the IP level testcases at the SoC level

As we need to use different languages like SystemVerilog or Verilog or C or Python to create the verification environment at different levels like IPs, Sub-Systems, Chips, and SoCs, we may not be able to reuse the IP level testcases/verification environments at the SoC level. So, usually, we rewrite/modify the IP level testcases to use them at the SoC level. This is completely a manual process and it’s more time-consuming.[^2]

 **Solutions**: 

1. We can define various random scenarios in UVM to model the firmware operation sequences which can eventually **drive the existing lower-level IP-UVM sequences**. This way we can scale-up the IP level random testcases to SoC level and do the exhaustive regression testing at the SoC level too, but still there may be many challenges of achieving coverage closure **due to redundant testcases and slow simulation speed**.[^3]
2. By **using PSS language** we can define the verification intent of any design IP/Chip/SoC and generate the verification environment in any language [testcases for simulation / emulation / FPGA prototyping] using the EDA tool.[^2]

## Hardware/Software Co-verification Using Virtual Platform with Hardware Emulation and Acceleration[^5]

### Slow to compile

One of the factors that affect selection of an emulation system is the compile (synthesis) times to put RTL into the box. What good does it do for emulation to provide results in minutes while compile times take hours. **Hardware emulators can accommodate any design size, but they require a long setup time and are relatively slow to compile, compared to simulation.**

### Difficult to debug without other verification methods

Also, the debug method based on the hardware emulation and acceleration is not as advanced as simulation. In some complex situations, you need to back to the slow simulation to debug after you find a bug.

[^1]: J. C. Chang, “Formal Veriﬁcation Along with Design for Transactional Models,” UCB.
[^2]: S. P. R, “PSS – Portable Test and Stimulus Standard,” Maven Silicon, Mar. 24, 2020. https://www.maven-silicon.com/blog/portable-test-and-stimulus-standard/ (accessed Jan. 11, 2021).
[^3]: S. P. R, “IP vs SoC Verification,” Maven Silicon, Mar. 17, 2020. https://www.maven-silicon.com/blog/ip-vs-soc-verification/ (accessed Jan. 11, 2021).
[^4]: E. M. Clarke, W. Klieber, M. Nováček, and P. Zuliani, “Model Checking and the State Explosion Problem,” in *Tools for Practical Software Verification*, Sep. 2011, pp. 1–30, doi: [10.1007/978-3-642-35746-6_1](https://doi.org/10.1007/978-3-642-35746-6_1).
[^5]: Mehta, Ashok B. "ASIC/SoC functional design verification." *Publ. Springer* (2018).
