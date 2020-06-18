---
layout: post
title: "[Emulate] SystemC and Its Simulation Kernel"
description: "This post introduces the components of SystemC, including Modules, Interfaces, Ports, Channels, Process and Events."
categories: [Emulate]
tags: [SystemC]
last_updated: 2020-06-13 23:41:00 GMT+8
excerpt: "This post introduces the components of SystemC, including Modules, Interfaces, Ports, Channels, Process and Events."
redirect_from:
  - /2020/06/12/
---

* Kramdown table of contents
{:toc .toc}
# SystemC and Its Simulation Kernel[^1]

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

+ Thread process is started once and only once by the simulator
  + Once it starts to execute, it is in complete control of the simulation
  + When terminated, it is gone forever
  + It typically contains an infinite loop and at least a wait
+ Two way to pass control back to the simulator
  + Wait statement – suspend the process
  + Exit – terminate the process
+ A thread process may suspend its execution by calling `wait()`
  + **Multi-cycle behavior** may be easily described using wait statement
  + Sometimes `wait()` is invoked indirectly
    + For instance, a blocking read/write to `sc_fifo` may invoke `wait()`
+ A thread process explicitly keeps its state of execution
  + The thread remembers the suspension point and all local variables
  + The thread starts from the suspension point when being resumed
+ The gain in expressiveness comes with a cost in performance
  + Process suspension and resumption need coroutines
  + Switching between coroutines (context switch) is expensive
+ `SC_CTHREAD`
  + A variation of `SC_THREAD` triggered at every clock edge

##### Syntax of Wait Statements

+ `wait(time)`;
+ `wait(event)`;
+ `wait(event 1 | event2)`; //any of these
+ `wait(event 1 & event2)`; //all of these
+ `wait(timeout, event)`; //event with timeout
+ `wait(timeout, event 1 | event2)`//any event with timeout
+ `wait(timeout, event 1 & event2)`//any event with timeout
+ `wait()`; // static sensitivity

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200613231134.png" alt="Thread Processes and Wait Statements" style="zoom:67%;" />

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200613232330.png" style="zoom:67%;" />

#### `SC_METHOD`

+ Similar to the Verilog always@ block
  + When triggered, the body is executed from the beginning to the end
+ It does not keep an implicit execution state
  + A locally declared variable is not permanent
+ It is called based on dynamic or static sensitivity

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200613232446.png" style="zoom:67%;" />

+ The `next_trigger()` temporarily set a sensitivity list
+ The `next_trigger()` may be called repeatedly
  + Each invocation encountered overrides the previous
  + The last one executed before a return determines the sensitivity
+ Unlike `wait()`, calling `next_trigger()` does not suspend the process
+ Syntax of next_trigger()
  + next_trigger(time); 
  + next_trigger(event); 
  + next_trigger(event 1 | event2) ; //any of these events
  + next_trigger(event 1 & event2) ; //all of these events are required
  + next_trigger(timeout, event); //event with timeout
  + next_trigger(timeout, event 1 | event2); //any of these events + timeout
  + next_trigger(timeout, event 1 & event2); //all of these events + timeout
  + next_trigger() ; //re–establish static sensitivity

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200613232819.png" style="zoom:67%;" />

### Events

+ An event represents a condition for triggering the processes
+ An event occurrence is usually associated with changes in processes or channels
  + The owner of the event reports the changes by calling `notify()`
  + The event object keeps a list of processes that are sensitive to it
    + Inform the scheduler of which processes to trigger
+ Events provide synchronization between processes

+ A SystemC event is the occurrence of an `sc_event` notification
  + Cause processes that are sensitive to it to be triggered
+ An event has no duration (like an impulse)
  + To observe an event, the observer must be watching for the event
  + If an event occurs, and no processes are waiting to catch it, the event goes unnoticed
+ An event has no value
  + It is illegal to test an event for true or false
  + It is ok to test a Boolean set by the process that caused an event
+ Two actions to do with events
  + Wait for an event
  + Cause an event to occur

```c++
class sc_event { 
    public:
        sc_event();
        ~sc_event(); 
        void cancel(); 
        void notify(); 
        void notify(const sc_time& ); 
        void notify(double, sc_time_unit );
        sc_event_or_list& operator | (const sc_event& ) const; 
        sc_event_and_list& operator & (const sc_event& ) const;
    private: 
        sc_event (const sc_event&); 
        sc_event& operator = ( const sc_event& ); 
}
```

#### Event Notification

+ Immediate notification - `event.notify()`
  + Processes waiting for the event are immediately moved from the wait pool into the ready pool for execution **(before the update)**
+ Delta-delayed notification - `event.notify(SC_ZERO_TIME) `
  + Processes waiting for a delayed notification will be executed on the next delta-cycle **(after the update)**
+ Timed notification - `event.notify(10, SC_NS) `
  + Timed events are scheduled to occur at some time in the future
+ Remarks
  + If a signal has had a new value written to it, processes triggered by immediate notification will see the old value of the signal, whereas processes triggered by delta-delay notification will see the new value
+ A pending delayed event notification may be canceled
  + Immediate event cannot be canceled

## Example

### Asynchronous Bus

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200613234528.png" alt="Asynchronous Bus" style="zoom:67%;" />

### Produce and Consumer

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200613235018.png" alt="Produce and Consumer" style="zoom:67%;" />

### Initiator and Target

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200613235200.png" alt="Initiator and Target" style="zoom:67%;" />

[^1]: [IOC5080(5940) System Model Design and Verification](http://mapl.nctu.edu.tw/course/ESL/index.php), Department of Computer Science, National Chiao-Tung University