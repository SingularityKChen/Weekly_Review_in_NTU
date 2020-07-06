---
layout: post
title: "[CodeStudy] The Implementation of TLM Simple Bus in SystemC"
description: "The study of the Implementation of TLM Simple Bus in SystemC"
categories: [CodeStudy]
tags: [SystemC, TLM]
last_updated: 2020-07-01 14:48:00 GMT+8
excerpt: "Study the code: the implementation of timed TLM in SystemC"
redirect_from:
  - /2020/07/01/
---

* Kramdown table of contents
{:toc .toc}
# The Implementation of TLM Simple Bus in SystemC[^1]

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200705110657.png" alt="SystemC 2.0 transaction-level model" style="zoom:50%;" />

In this project, there are the following c++ files:

+ `simple_bus_main.cpp`: 
+ `simple_bus_test.h`: the testbench, descriptions the instance of the modules and the inter connections.
+ `simple_bus_master_blocking.h`: the blocking master's ports, the constructor as well as private values.
+ `simple_bus_master_blocking.cpp`: the blocking master's process function.
+ `simple_bus_master_non_blocking.h`: 
+ `simple_bus_master_non_blocking.cpp`: the process function.
+ `simple_bus_master_direct.h`: 
+ `simple_bus_master_direct.cpp`: the process function.
+ `simple_bus_slow_mem.h`: including the implementations of the slave interface.
+ `simple_bus_types.h`: 
+ `simple_bus_blocking_if.h`: the blocking interface
+ `simple_bus_direct_if.h`: the direct interface
+ `simple_bus_non_blocking_if.h`: the non blocking interface
+ `simple_bus_request.h`:
+ `simple_bus_slave_if.h`: the slave interface
+ `simple_bus.h`: 
+ `simple_bus.cpp`: including the implementations of master interfaces.
+ `simple_bus_fast_mem.h`:  including the implementations of the slave interface.
+ `simple_bus_arbiter.h`: 
+ `simple_bus_arbiter_if.h`: the arbiter interface
+ `simple_bus_arbiter.cpp`: 
+ `simple_bus_types.cpp`: 
+ `simple_bus_tools.cpp`:

## Implementation of Masters



## Implementation of Bus

### Dynamic Sensitive

```c++
  wait(request->transfer_done); // event, dynamic
  wait(clock->posedge_event()); // statically sensitive (activated every cycle)
```



## Implementation of Slaves



[^1]: Ric Hilderink, Synopsys, Inc., 2001-10-11