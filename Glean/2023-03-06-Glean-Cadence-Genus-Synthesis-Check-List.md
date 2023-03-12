---
layout: post
title: "[Glean] Cadence Genus Synthesis Check List"
description: "Here lists several messages that should be checked from the Genus synthesis log file to make sure there is no error and mismatch between the simulation and synthesis results."
categories: [Glean]
tags: [Cadence, Genus, Synthesis]
last_updated: 2023-03-06 13:35:00 GMT+8
excerpt: "Here lists several messages that should be checked from the Genus synthesis log file to make sure there is no error and mismatch between the simulation and synthesis results."
redirect_from:
  - /2023/03/06/
---

* Kramdown table of contents
{:toc .toc}

# Cadence Genus Synthesis Check List

Here lists several messages that should be checked from the Genus synthesis log file to make sure there is no error and mismatch between the simulation and synthesis results.

## CDFG

- [ ] Referenced signals are not added in sensitivity list. This may cause simulation mismatches between the original and the synthesized design. [CDFG-360]

    > Add missing reference signals in the sensitivity list or use '*' to add all the signals in the sensitivity list. Example : always @(a or b) (OR) always (*))

- [ ] Signal is not referenced within the process or block, but is in the sensitivity list. [CDFG-361]
    
    > This message indicates that a process contains a signal that clocks or gates other signals in the process. However, this signal does not appear in either a wait statement or the process sensitivity list.

- [ ] Bitwidth mismatch in assignment. [CDFG-372]
    
    > Review and make sure the mismatch is unintentional.

- [ ] Unreachable statements for case item. [CDFG-472]
- [ ] Loop condition is always false. [CDFG-485]
- [ ] Unused module input ports. [CDFG-500]
    
    > In port definition within the module, the input port is not used in any assignment statements or conditional expressions for decision statements.

- [ ] Removing unused register. [CDFG-508]
    
    > Genus removes the flip-flop or latch inferred for an unused signal or variable. To preserve the flip- flop or latch, set the `hdl_preserve_unused_registers` attribute to true or use a pragma in the RTL.

- [ ] Replaced logic with a constant value. [CDFG-771]
- [ ] Removed unused code identified during constant propagation. [CDFG-772]
- [ ] Signal or variable has multiple drivers. [CDFG2G-622]
    
    > This may cause simulation mismatches between the original and synthesized designs.


## ELABUTL

- [ ] Undriven module output port. [ELABUTL-123]
- [ ] Unconnected instance input port detected. [ELABUTL-124]
    
    > Run `check_design` to check 'Undriven Port(s)/Pin(s)' section for all unconnected instance input ports. It is better to double confirm with designer these unconnected instance input port are expected.

- [ ] Undriven signal detected. [ELABUTL-125]
- [ ] Undriven module input port. [ELABUTL-127]
- [ ] Undriven module output port. [ELABUTL-128]
- [ ] Unconnected instance input port detected. [ELABUTL-129]
- [ ] Undriven signal detected. [ELABUTL-130]
- [ ] Undriven module input port. [ELABUTL-131]
- [ ] Unused instance port. [ELABUTL-132]

## GLO

- [ ] Replacing a flip-flop with a logic constant 0. [GLO-12]
- [ ] Replacing a flip-flop with a logic constant 1. [GLO-13]
- [ ] Deleting sequential instances not driving any primary outputs. [GLO-32]
- [ ] Deleting instances not driving any primary outputs. [GLO-34]
- [ ] Replacing the synchronous part of an always feeding back flip-flop with a logic constant. [GLO-45]

## VLOGPT

- [ ] Ignoring unsynthesizable construct. [VLOGPT-37]
    
    > For example, the following constructs will be ignored: `initial block` - `final block` - `program block` - `property block` - `sequence block` - `covergroup` - `checker block` - `gate drive strength` - `system task enable` - `reg declaration with initial value` - `specify block`.

- [ ] Cannot open files. [VLOGPT-650]
