---
layout: post
title: "[Emulate] TLM API 1.0 in SystemC"
description: "This is the 7th SystemC lecture in National Chiao-Tung University."
categories: [Emulate]
tags: [SystemC, TLM]
last_updated: 2020-07-03 20:40:00 GMT+8
excerpt: "TLM API 1.0 in SystemC. Including core interfaces and standard channels."
redirect_from:
  - /2020/07/03/
---

* Kramdown table of contents
{:toc .toc}
# TLM API 1.0 in SystemC[^1]

## Introduction

### What Dose TLM API 1.0 Provide and not?

+ What does TLM API 1.0 provide?
  + Define **core interfaces** and **standard channels** for communication
    + Users can design their own channels implementing core interfaces
+ What is not defined in TLM API 1.0?
  + The **content of the transactions**
    + The task left for the TLM 2.0 that is now under development
  + The constraints on the implementation of channels

### High Level Goals

+ Provide a common way of TLM modeling at different levels
  + A "recipe" that allows new users to get started quickly
+ Help users avoid problems
  + Concurrency, memory leaks, pointer aliasing, SEGFAULTS, etc.
+ Achieve efficiency in space and time without sacrificing clarity and safety
  + A set of interfaces that map to **both HW and SW, and across HW/SW**
+ Create interfaces and methods that support IP reuse
  + within a project from one abstraction level to another
  + across projects
+ Promote IP interoperability through standard interfaces
+ Promote a design style encapsulating functions in a polymorphous way
  + Operations on classes are independent of the class
  + An arbiter that is independent of the transaction data type

### General Strategy

+ Establish a fundamental set of TLM interfaces
+ Core interfaces
  + Unidirectional interfaces - send data in one direction
    + `tlm_put_if<T>`, `tlm_get_if<T>`, `tlm_peek_if<T>`
  + Bidirectional interfaces - send data in both directions 
    + `tlm_transport_if<REQ, RSP>`
    + `tlm_maser_if<REQ, RSP>`, `tlm_slave_if<REQ, RSP>`
+ Standard channels
  + `tlm_fifo<T>`, `tlm_req_rsp_channel<REQ,RSP>`
  + `tlm_transport_channel<REQ,RSP>`

### Communication Types

+ Bidirectional communication
  + A read transaction across a bus is bidirectional
  + Burst write with a completion bus status is bidirectional
+ Unidirectional communication
  + Place read address on bus is unidirectional
  + Send IP packet is unidirectional
+ **Complex protocol** can be broken down into a sequence of **bidirectional or unidirectional transfers**
+ A complex bus with address, control and data phases may look like **a simple bidirectional read/write bus** at a high level of abstraction, but more like a sequence of **pipelined unidirectional transfers** at a more detailed level

### Interface Styles

+ Similar to interfaces of `sc_fifo`
+ Data transfer is effectively done by **Pass-by-Value mechanism**
  + Use “const &” for interface parameters
  + Data send back to caller as return value
  + Never use pointer
  + Never use non const & for inbound data
+ Inbound data is always passed by `(const &)`
  + `bool nb_put(const &)`
+ Outbound data is **returned by value**
  + There is data to return
    + `T get()`
  + There may not be data to return
    + Return the status and pass in a `(non-const &)`
    + `bool nb_get(T&)`

+ Effectively Pass-by-Value data transfer
  + Pros
    + Eliminate problems of pointers and dynamic memory allocation
    + Help eliminate problems of race conditions
    + Help you reason about concurrent systems
    + Enable use of C++ smart containers and handles
    + Simple, clear lifetime and ownership of objects
  + Cons
    + Naïve passing of large objects may lead to performance problems
      + E.g., passing larger vectors or arrays of data by value

```c++
//blocking write of sc_fifo 
// call by reference
template <class T> 
inline void sc_fifo<T>::write( const T& val_ ) // call by reference
{
  while( num_free() == 0 ) 
    sc_core::wait( m_data_read_event );
  m_num_written ++; 
  buf_write( val_ ); // call by reference
  request_update();
}
```

```c++
// pass by value
template <class T> 
inline bool sc_fifo<T>::buf_write( const T& val_ ) 
{
  if( m_free == 0 ) { return false; }
  m_buf[m_wi] = val_; // pass by value
  m_wi = ( m_wi + 1 ) % m_size; 
  m_free --;
  return true;
}
```



## Core Interfaces

### Introduction of Core Interfaces

+ Developed based on `sc_fifo` interface
+ Primary unidirectional interfaces
  + `tlm_put_if<T>` (blocking+non-blocking)
  + `tlm_get_if<T>` (blocking+non-blocking)
  + `tlm_peek_if<T>` (blocking+non-blocking)
+ Primary bidirectional interface
  + `tlm_transport_if<REQ, RSP> (blocking)`
+ Channel specific bidirectional interfaces
  + `tlm_master_if<REQ, RSP>` (blocking+non-blocking)
  + `tlm_slave_if<REQ, RSP>` (blocking+non-blocking)
  + Useful for `tlm_req_rsp_channel` and `tlm_transport_channel`

### Key Terms

+ Peek
  + Peek reads most recent valid value
  + Similar to read to a variable or signal
+ Put/Get
  + Put queues data and moves a transaction from initiator to target
  + Get consumes data and moves a transaction from target to initiator
  + Similar to write/read from a FIFO
+ Master/Slave
  + Master initiates activity by issuing a request
  + Slave passively waits for requests and returns a response
+ Blocking
  + Mean method implementation might call `wait()`
  + Methods may not return immediately
  + Methods can be **called only from `SC_THREAD `processes**
+ Non-blocking
  + Mean method implementation can **never** call `wait()`
  + Methods return immediately with a bool indicating the success
  + Methods can be called from `SC_METHOD` or `SC_THREAD` processes

### Hierarchy of Core Interfaces

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200703194458.png" alt="Hierarchy of Core Interfaces" style="zoom:50%;" />

### Unidirectional Interfaces

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200703194639.png" alt="Unidirectional Interfaces" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200703195021.png" alt="tlm_peek_if" style="zoom:50%;" />

+ Non-blocking interfaces may fail and thus must **return a bool value**
+ Polling-based usage of non-blocking put, get, and peek
  + `nb_can_put/get/peek` enquires whether a transfer will be successful
+ **Interrupt-based usage** of non-blocking put, get and peek
  + Event functions enables an `SC_THREAD` to wait until it is likely that the
    access succeeds or a `SC_METHOD` to be woken up

### Example of Bidirectional Interfaces

+ Model transactions with a tight **1-to-1**, **non pipelined** binding between the request and the response
  + A merger between the blocking get and put functions
+ Useful for modeling from a software programmer’s point of view
  + A read with an address going in and the read data coming back

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200703195322.png" alt="Example of Bidirectional Interfaces 1" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200703195404.png" alt="Example of Bidirectional Interfaces 2" style="zoom: 67%;" />

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200703195619.png" alt="tlm_master_if and tlm_slave_if" style="zoom:67%;" />

### Hardware Implication

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200703195738.png" alt="Hardware Implication" style="zoom:50%;" />

## Standard Channels

### Remarks

+ Users can and should design their own channels implementing some or all of these core interfaces
+ They can implement them directly in the target using `sc_export`
+ The transport function in particular will often be directly implemented in a target when used to provide fast programmers view models for software prototyping

### `tlm_fifo<T>`

+  Implement all the unidirectional TLM interfaces based on `sc_fifo`
+ Support request_update/update mechanism
+ Support FIFO size of 0 and infinite

### `tlm_req_rsp_channel<REQ, RSP>`

+ One `tlm_fifo` for the request going from initiator to target
+ One `tlm_fifo` for the response being moved from target to initiator
+ The FIFOs in `tlm_req_rsp_channel` can be of arbitrary size

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200703200156.png" alt="tlm_req_rsp_channel" style="zoom:50%;" />

### `tlm_transport_channel`

+ **1-to-1, non pipelined** binding between request and response
+ The FIFOs in `tlm_req_rsp_channel` must be of **size one**
+ Used to **bridge time and untimed TLMs**

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200703200231.png" alt="tlm_transport_channel" style="zoom:50%;" />

## Background Information

### Key Terms

+ TLM target port
  + `sc_export` bound to a TLM core interface
+ TLM initiator port
+ `sc_port` bound to a TLM core interface
+ TLM target 
+ Module instantiating at least one TLM target port
+ TLM initiator
+ Module instantiating at least one TLM initiator port
+ TLM transactions
+ Data structures transferred by TLM core interface
+  System initiator
  + Example: a CPU issuing read/write requests
  + Master
+ System target
  + Example: a memory serving read/write requests
  + Slaver
+ System transactions
  + Example: a read/write operation from a CPU to a memory
+ Corollary
  + A system initiator is always a TLM initiator
  + A system component might be both initiator and target
  + A SystemC module might be both a TLM initiator and target
  + A system transaction might be built from a sequence of several TLM
    transactions

### Compare of `sc_export` and `sc_port`

+ `sc_port` (output)

  + Declare interfaces **required** at a module boundary
  + `sc_port<IF>` of a module **requires** that interface from the outside

+ `sc_export` (input)

  + Declare interfaces **provided** at a module boundary
  + `sc_export<IF>` of a module **provides** that interface to the outside
  + Improve speed by reducing the number of threads

  <img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200703202955.png" alt="Compare of sc_export and sc_port" style="zoom:67%;" />

  + Allow `sc_ports` to connect to more than one implementation of the same interface in the same top level block

  <img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200703203418.png" alt="Allow sc_ports to connect to more than one implementation of the same interface in the same top level block" style="zoom:50%;" />

  + Allow a different transport function for each protocol

  <img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200703203447.png" alt="Allow a different transport function for each protocol" style="zoom:50%;" />

  + A thread in a low level sub module inside an initiator directly calls a method in low level sub module in a target

  <img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200703203335.png" alt="A thread in a low level sub module inside an initiator directly calls a method in low level sub module in a target" style="zoom:50%;" />



### Improve Reusability

+ Channels/`sc_export` **offering more** than actually required by `sc_port` are still “Plug Compatible” [Lecture pp18-19]
+ C++ implicit conversions to base classes
+ Having **a hierarchy of interfaces** is the key to enable this feature
+ **Require the most minimal interfaces** for a given situation
+ Use `sc_port<tlm_nonblocking_get_if<T> >` rather than `sc_port<tlm_get_if<T> >`
+ **Provide the maximal interfaces** for a given situation
+ Use `sc_export<tlm_get_if<T> >` rather than `sc_export<tlm_blocking_get_if<T> >`

[^1]: [IOC5080(5940) System Model Design and Verification](http://mapl.nctu.edu.tw/course/ESL/index.php), Department of Computer Science, National Chiao-Tung University