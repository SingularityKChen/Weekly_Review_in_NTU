---
layout: post
title: "[Emulate] Refinement of Computation and Communication"
description: "This post introduces Refinement of Computation and Communication in SystemC."
categories: [Emulate]
tags: [SystemC, Refinement, Adapters, Converters]
last_updated: 2020-06-24 21:45:00 GMT+8
excerpt: "This post introduces Refinement of Computation and Communication in SystemC. Including different kinds of communication refinement, such as channel refinement, module refinement, hw-hw refinement, sw-sw refinement, hw-sw refinement. It also introduces the steps in communication refinement."
redirect_from:
  - /2020/06/19/
---

* Kramdown table of contents
{:toc .toc}
# Refinement of Computation and Communication[^1]

## Introduction

### Communication Refinement

+ Expand an abstract communication into an actual implementation
  + Example: transition from `sc_fifo` to `hw_fifo`
  + Implementation can be either software or hardware
    + Hardware: **a shift register** or **a RAM** with some control logic
    + Software: **a certain memory range** with a modulo addressing scheme
+ No simple one-size-fits-all approach
+ Key principles
  + Use well-defined and well-understood interfaces
  + Intelligent use and reuse of adapters and converters

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200624210258.png" alt="Example of Communication Refinement" style="zoom:67%;" />

### Steps in Communication Refinement(Important)

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200624210554.png" alt="Steps in Communication Refinement" style="zoom:67%;" />

### Adapters – Channel Refinement

+ A **hierarchical channel** that translates the interface it implements into port accesses bound to a different interface
  + Example: the `read()`/`write()` methods in the `hw_fifo_wrapper` can be encapsulated in an adapter, respectively
+ Adapters can be easily reused and a small library of adapters is sufficient
  + Typically only a finite number of different interfaces are used
+ Two things that can be done with an adapter for refinement
  + Introduce an additional level of hierarchy
    + Wrap the refined channel and the adapters connected to it
  + Merge the adapters into the calling modules
    + Move ports, processes, methods, and data fields to the calling processes 
    + Replace calls to the adapter’s methods with the new member functions

### Converters – Module Refinement

+ A **module** that translates a set of ports to another in order to make the refined module and communication channel fit together
  + The refined model could be a piece of an existing IP and have a different set of ports than its abstract counterpart
+ Things that can be done with an converter for refinement
  + Wrap a refined module and a converter as a new module that matches the channel’s footprint
  + Merge an adapter to form a new, refined converter
    + A converter may accesses an adapter via the adapter’s interface
+ Converters versus Adapters
  + Converters tend to be a component in **the final implementation**
  + Adapters are often **temporal objects**

### General Process of Refinement(Important)

1. Select and Replace
   + Select a refined implementation for a component
   + Replace the original component with the refined implementation
2. Insert adapters or converters (optional)
   + Make channel and module play together
   + Adapters and converters should be stored in an accessible library
3. Analyze the I/O functionality and performance
   + Verify the functionality and system performance
4. Flatten (optional)
   + Ungroup the hierarchy of the wrapped channel
   + Merge adapters and connected modules
5. Wrap (optional)
   + Wrap the refined channel and the set of connected adapters to create a new channel for software implementation
6. Merge (optional)
   + Refine an abstract communication protocol (transaction-level) towards a hardware implementation (pin-level)
   + Merge adapters into the calling modules (protocol inline)
7. Restructure control flow (optional)
   + High level module is generally described in a sequential manner using blocking `read()`/`write()`
     + first_output->write(some_data)
     + second_output->write(some_data)
   + Sequential execution might not be suited for hardware
   + One may want to refine control flow to exploit more parallelism
     + first_output->initiate_transmission(some_data)
     + second_output->initiate_transmission(some_data) 
     + wait_for_completion(first_port, second_port)
8. Analyze Implementation
   + Check both functionality and I/O performance
   + Analyze the partial or entire system for its implementation characteristics such as area, latency, clock frequency, and power dissipation

## Hardware-Hardware Communication Refinement (From Abstract Model to RTL)

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200624211139Hardware-Hardware%20Communication%20Refinement.png" alt="Hardware-Hardware Communication Refinement" style="zoom:67%;" />

### Implementation of Adapters

#### Source Codes

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200624212551Implementation_of_Adapters.png" alt="Implementation of Adapters" style="zoom:67%;" />

```c++
SC_MODULE(SOURCE) 
{
//output port 
  sc_fifo_out<int> out;
//The one and only process 
  void my_process(){ 
    int counter = 0; 
    while(1){
      counter++;
      output->write(counter);
    } 
  }
  C_CTOR(SOURCE) 
  {
    SC_THREAD(my_process) 
  } 
};

template<class T> class FIFO_WRITE_HS 
: public sc_module 
: public sc_fifo_out_if<T>
{
  public:
    sc_in_clk clk; 
    sc_out<T> data; 
    sc_out<bool> valid; 
    sc_in<bool> ready;

    //blocking write 
    void write(const T& x){ 
      data = x;//drive data line
      valid = true; //signal the data is valid 
      do{ //wait until data was read
        wait(clock->posedge_event()); 
      }while(ready.read()!=true); 
      valid = false;
    }
    //Provide dummy implementations for
    //unneeded sc_fifo_out<T> methods
    bool nb_write(const T&x) 
    {assert(0); return false;} 
    …………………………
};
```

#### Merging Adapters

+ Goal
  + To merge the adapters into the calling module for “protocol inline”
  + The result is a refined pin-level module
+ **Procedures** (Important)
  1. Copy and paste the adapter’s properties into the calling module
     + Ports, methods, data fields, and processes
     + Care must be taken to avoid name clash
  2. Remove the original port that was used to access the adapter
  3. Replace the template argument T with the actual data type 
  4. Create methods and constructors of the calling module
     + The adapter’s methods become new methods of the calling module
     + The adapter’s constructor must be merged into the calling module’s constructor
     + If the adapter has additional constructor arguments, these can usually be replaced with constant expressions
  5. Replace references to the adapter’s clock with accesses to the calling module’s clock

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200624212701Merging_Adapters.png" alt="Merging Adapters" style="zoom:67%;" />

### Implementation of Converters

Assume we select an existing IP HW_SINK for the SINK module

+ The IP has active low on the pins valid and ready
+ A converter is used to reverse the polarity of the “valid” and “ready”

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200624212939.png" style="zoom:67%;" />

### Control Flow Restructuring

+ One should use `fork` and `join` constructs to model parallel implementation for design space exploration
+ Restructure the control flow if the results look encouraging

#### Source Codes

```c++
void HW_SOURCE::my_process(){
  int counter = 0;
  while(1){
    counter++;
    first_output_data = counter;
    first_output_valid = true;
    second_output_data = counter/2;
    second_output_valid = true;
    do{
      wait(clock->posedge_event()); 
      if(first_output_ready.read()==true)
        first_output_valid = false;
      if(second_output_ready.read()==true)
        second_output_valid = false;
    }while(!(first_output_ready.read() && second_output_ready.read()));
  }
}
```

## Software-Software Communication Refinement (From SystemC to Plain C)

+ Refine SystemC models towards software implementations
  + **Map the SystemC model into software implementation**
  + Convert the SystemC design into C++ or even C codes
  + Get rid of SystemC elements
+ OS support is required for mapping the SystemC processes into concurrent threads
+ Refinement process is **architecture- and OS-dependent**
  + Primitive elements of software-based communication 
    + Memory and a set of access methods
    + Inter-process synchronization primitives offered by the OS
+ Support for modeling software running on the RTOS will be improved in future versions of SystemC

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200624220059.png" style="zoom:67%;" />

## Hardware-Software Communication Refinement
+ Hardware Part: From Abstract Model to RTL
+ Software Part: From SystemC to Plain C

### Communication refinement: XBAR

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200624220749.png" alt="Hardware-Software Communication Refinement" style="zoom:67%;" />

#### Implementation of XBAR

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200624224219.png" alt="Communication refinement XBAR" style="zoom:67%;" />

#### What Are to Be Explored?

+ Interconnect topology between modules
  + Hub and spoke architecture
  + Cross bar switch
+ Arbitration policy and prioritized accesses
+ FIFO sizes associated with each module
+ Resource locking
+ Wait states
+ Timing and performance 
+ ……..

### Platform Refinement

#### Hardware Part

+ The platform is refined after the system has been analyzed at a higher level of abstraction
  + A continuous refinement process
  + Selection from a number of available IP blocks
    + Busses, memory sub-system, arbiters, bridges, and so on
+ It is advisable **not** to make a giant step towards a pin-level model
+ Model the platform - possibly in a cycle-accurate way - at the transaction level
+ **What are to be explored?** 
  + The number of buses 
  + The features of the buses
  + Connection topology
  + Decision of master and slave
  + Modes of transmission
    + Send data immediately by a single read/write
    + Cache data and send it as a burst read/write
    + Initiate a split transaction (slave)
  + Burst length in case of burst read/write
  + Bus arbitration policy
  + Software/Firmware/Hardware implementation

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200624224416.png" style="zoom:67%;" />

#### Software Part

+ Refine the software down to the bus transaction level
+ Make sure the software will interact properly with the hardware

[^1]: [IOC5080(5940) System Model Design and Verification](http://mapl.nctu.edu.tw/course/ESL/index.php), Department of Computer Science, National Chiao-Tung University