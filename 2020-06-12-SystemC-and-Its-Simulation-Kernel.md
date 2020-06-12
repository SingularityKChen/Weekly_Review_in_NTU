---
layout: post
title: "[Weekly Review] SystemC and Its Simulation Kernel"
description: "This post introduces the components of SystemC, including Modules, Interfaces, Ports, Channels, Process and Events."
categories: [WeeklyReview]
tags: [SystemC]
last_updated: 2020-06-12 22:53:00 GMT+8
excerpt: "This post introduces the components of SystemC, including Modules, Interfaces, Ports, Channels, Process and Events."
redirect_from:
  - /2020/06/12/
---

* Kramdown table of contents
{:toc .toc}
# SystemC and Its Simulation Kernel

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200612002808.png" alt="TLM-based SoC Design" style="zoom: 50%;" />

## SystemC Language Architecture

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200612215229.png" alt="SystemC Language Architecture" style="zoom: 50%;" />

## SystemC Scheduler

+ Step 1: Elaboration
  + Creating instances of clocks, design modules, and channels
  + Establishes hierarchy and initializes the data structures
  + Register processes and perform the connections between modules
+ Step 2: Initialization
  + Each process is executed once (SC_METHOD) or until a synchronization point (i.e., a wait) is reached (for SC_THREAD)
+ Step 3: Evaluation (Delta cycle=c0, time = t0)
  + Select a process being ready to run and resume for its execution
  + If a process P has been executed in the current phase, it will be triggered again if a controlling event is notified immediately
+ Step 4: Update (Delta cycle=c0, time = t0)
  + Execute any pending calls to update() resulting from the calls to request_update() in the evaluation phase
  + The update of channels could generate notification of events
+ Step 5: Delta-delay processing (Delta cycle=c0+1, time = t0)
  + If there are pending delta-delay notifications, determine which processes are ready to run and go to the evaluation phase (Step 2)
  + Simulation time is not advanced
+ Step 6: Simulation time advance (Delta cycle=c0, time = t1)
  + Advance the simulation time to the earliest pending timed notification
  + Determine which processes are ready to run due to the events that have pending notifications at the current time
  + If there are no more timed notification, the simulation is finished

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200612215332.png" alt="SystemC Scheduler" style="zoom:50%;" />

## SystemC Components

### System View in SystemC

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200612215903.png" alt="System View in SystemC" style="zoom:50%;" />

### Modules

+ Basic building blocks
  + Break complex systems into smaller pieces
  + Hide internal data representation and algorithms from other modules
+ Composition of a module
  + Ports
  + Processes
  + Internal data and channels
  + Other modules

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200612220450.png" alt="Example two-input adder" style="zoom: 67%;" />

### Interfaces, Ports, Channels

+ Classical hardware modeling
  + Use hardware signals as the medium for communication and synchronization between processes
+ SystemC modeling
  + Use interfaces, ports, and channels to provide for the flexibility and high level of abstraction needed
+ Interfaces, ports and channels
  + Channel is the **workhorse** for holding and transmitting data
  + Interface is a **window** into a channel describing the set of operations
  + Port acts as a **proxy** object that facilitate the access of channel
+ The port is bound to the channel through the interface
  + The SystemC library connects each port to a designated channel

#### Interfaces

+ An interface defines a set of functions
  + Specify only the signature of each function
  + The operation’s name, parameters, and return value
  + All interfaces must be derived from `sc_interface`
+ One can easily plug in different channel implementations with the same interfaces for channel refinement

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200612220723.png" alt="Interfaces" style="zoom:67%;" />

#### Ports

+ Ports correspond to interfaces
  + Each kind of port must specify the interface to which it corresponds
  + Example: `sc_port<sc_signal_in_if<int> > portA`;
+ Interface method calls (IMC)
  + `portA->read()`
  + `read()` is the interface function defined in `sc_signal_in_if`
+ All ports must be derived from `sc_port_base`

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200612221148.png" alt="Ports" style="zoom: 67%;" />

#### Channels

+ Channels define how interface functions are performed
+ Different channels may implement an interface in different ways
+ A channel may implement more than one interface
+ Example:
  + `sc_signal<T>` implements both `sc_signal_in` and `sc_signal_inout`

##### Primitive Channels

+ Definition
  + Do not contain other structure
  + Support Request-Update Mechanism
+ Request-Update Mechanism
  + Allow simultaneous accesses to a channel to be correctly simulated
  + Simulation results should not depend on the order in which simultaneous actions are executed
  + How?
    + Any change to the state of the channel will not take effect until all currently activated processes are executed
    + Employ the two-phase scheme of the kernel to serialize actions
  + Evaluation phase
    + Call `request-update()` to instruct the scheduler to place the channel in an update queue
  + Update phase
    + The scheduler takes items from the update queue and calls the `update()` method associated with each item

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200612225008.png" alt="Primitive Channels in SystemC" style="zoom:67%;" />

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200612224543.png" alt="Primitive Channels" style="zoom:67%;" />

##### Hierarchical Channels

+ Implement one or more interfaces and have internal processes
  + Modeling of complex communication structures such as on-chip-bus
  + Refinement of primitive channels
    + One may refine the FIFO buffer by including handshakes which may be encapsulated inside the `read()` and `write() `functions

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200612225110.png" alt="Hierarchical Channels" style="zoom:67%;" />

### Process

+ The basic unit of functionality
  + A process is a member function of the module
    + Note that a member function is not necessarily a process
  + A declaration in the module’s constructor is required
    + Register the member function with the simulation kernel
+ Two major types of processes
  + `SC_THREAD`
  + `SC_METHOD`

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200612222700.png" alt="Process" style="zoom:67%;" />

#### `SC_THREAD`



#### `SC_METHOD`



### Events

+ An event represents a condition for triggering the processes
+ An event occurrence is usually associated with changes in processes or channels
  + The owner of the event reports the changes by calling `notify()`
  + The event object keeps a list of processes that are sensitive to it
    + Inform the scheduler of which processes to trigger
+ Events provide synchronization between processes