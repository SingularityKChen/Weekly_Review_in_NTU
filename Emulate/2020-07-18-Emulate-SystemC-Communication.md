---
layout: post
title: "[Emulate] SystemC Communication Ports"
description: "SystemC Communication ports. Including chapter 11 and 12 of the book SystemC from the Ground Up."
categories: [Emulate]
tags: [SystemC]
last_updated: 2020-07-18 11:45:00 GMT+8
excerpt: "SystemC Communication ports. Including chapter 11 and 12 of the book SystemC from the Ground Up. Including the concepts of interface, port and channel."
redirect_from:
  - /2020/07/18/
---

* Kramdown table of contents
{:toc .toc}
# SystemC Communication Ports[^1]

## Communication: The Need for Ports

There are two concerns: safety and ease of use. 

Safety is a concern because all activity occurs within processes, and care must be taken when communicating between processes to **avoid race conditions**. Events and channels are used to handle this concern.

Ease of use is more difficult to address. SystemC takes an approach that lets modules **use channels** inserted between the communicating modules with ports. A port is **a pointer to a channel** outside the module.

This separation of access is managed using a concept known as an interface, which is described in the next section.

## Interfaces: C++ and SystemC

**An abstract class** is a class that is never used directly, but it is used only via derived subclasses. Partly to enforce this concept, abstract classes usually contain **pure virtual functions**. Pure virtual functions are not allowed to provide an implementation in the abstract class where they are defined as pure.

Pure virtual functions are identified by

+ the keyword virtual
+ the =0;

```c++
class my_interface { 
    public: 
    virtual void write(unsigned addr, int data) = 0; 
    virtual int read(unsigned addr) = 0;
};
```

You can think of an interface **as the application programming interface (API)** to a set of derived classes.

> DEFINITION: A SystemC interface is an abstract class that inherits from `sc_interface` and provides only **pure virtual declarations of methods** referenced by SystemC channels and ports. No implementations or data are provided in a SystemC interface.
>
> DEFINITION: A SystemC channel is a class that inherits from either `sc_channel` or from `sc_prim_channel`, and the channel **should inherit and implement one or more SystemC interface** classes. A channel implements all the pure virtual methods of the inherited interface classes.

## Simple SystemC Port Declarations

> DEFINITION A SystemC port is a class templated with and inheriting from a SystemC interface. Ports allow access of channels across module boundaries.

SystemC ports are always defined within the module class definition.

## Many Ways to Connect

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200718110256.png" alt="Many Ways to Connect" style="zoom:50%;" />

## Port Connection Mechanics

There are two syntaxes for connecting ports: by name and by position.

```c++
mod_inst.portname(channel_instance); // Named 
mod_instance(channel_instance,…); // Positional
```

### Interconnect example

```c++
//FILE: Rgb2YCrCb.h 
SC_MODULE(Rgb2YCrCb) {
    sc_port<sc_fifo_in_if<RGB_frame> > rgb_pi;
    sc_port<sc_fifo_out_if<YCRCB_frame> > ycrcb_po;
};

//FILE: YCRCB_Mixer.h 
SC_MODULE(YCRCB_Mixer) {
    sc_port<sc_fifo_in_if<float> > K_pi; 
    sc_port<sc_fifo_in_if<YCRCB_frame> > a_pi, b_pi;
    sc_port<sc_fifo_out_if<YCRCB_frame> > y_po;
};

//FILE: VIDEO_Mixer.h 
SC_MODULE(VIDEO_Mixer) { 
    // ports 
    sc_port<sc_fifo_in_if<YCRCB_frame> > dvd_pi; 
    sc_port<sc_fifo_out_if<YCRCB_frame> > video_po; 
    c_port<sc_fifo_in_if<MIXER_ctrl> > control; 
    sc_port<sc_fifo_out_if<MIXER_state> > status; 
    // local channels 
    sc_fifo<float> K; 
    sc_fifo<RGB_frame> rgb_graphics;
    sc_fifo<YCRCB_frame> ycrcb_graphics; 
    // local modules 
    Rgb2YCrCb Rgb2YCrCb_i; 
    YCRCB_Mixer YCRCB_Mixer_i; 
    // constructor 
    VIDEO_Mixer(sc_module_name nm); 
    void Mixer_thread();
};
```

Local modules communicated via local channels.

```c++
// Example of port interconnect by name
SC_HAS_PROCESS(VIDEO_Mixer); 
VIDEO_Mixer::VIDEO_Mixer(sc_module_name nm) 
    : sc_module(nm)
    , Rgb2YCrCb_i(“Rgb2YCrCb_i”) 
        , YCRCB_Mixer_i(“YCRCB_Mixer_i”) 
    {
        // Connect
        Rgb2YCrCb_i.rgb_pi(rgb_graphics); 
        Rgb2YCrCb_i.ycrcb_po(ycrcb_graphics); 
        YCRCB_Mixer_i.K_pi(K); 
        YCRCB_Mixer_i.a_pi(dvd_pi); 
        YCRCB_Mixer_i.b_pi(ycrcb_graphics); 
        YCRCB_Mixer_i.y_po(video_po);
}
```

Although slightly more code than the positional notation, the named port syntax is more robust, and tools exist to reduce the typing tedium.

```c++
// Example of port interconnect by position
SC_HAS_PROCESS(VIDEO_Mixer); 
VIDEO_Mixer::VIDEO_Mixer(sc_module_name nm) 
    : sc_module(nm) 
    {
        // Instantiate 
        Rgb2YCrCb_iptr = new Rgb2YCrCb( "Rgb2YCrCb_i");
        YCRCB_Mixer_iptr = new YCRCB_Mixer( "YCRCB_Mixer_i"); 
        // Connect
        (*Rgb2YCrCb_iptr)( rgb_graphics 
                          ,ycrcb_graphics); 
        (*YCRCB_Mixer_iptr)( K 
                            ,dvd_pi
                            ,ycrcb_graphics 
                            ,video_po);
}
```

The problem with positional connectivity is that of keeping the ordering correct. we recommend avoiding the positional syntax entirely, and always **using a named port approach.**

## Accessing Ports From Within a Process

Using the operator-> when accessing ports.

```c++
void VIDEO_Mixer::Mixer_thread() { 
    …
    switch (control->read()) { 
        case MOVIE: K.write(0.0f); break; 
        case MENU: K.write(1.0f); break; 
        case FADE: K.write(0.5f); break; 
        default: status->write(ERROR); break;
    } 
    …
}
```

Ports feel and behave as if they were pointers.

## The SystemC Port Array and Port Policy

The `sc_port<T>` provides additional template parameters we have not yet discussed: the array size parameter and the port policy parameter.

```c++
sc_port<interface[,N[,POL]]> portname; 
// N=0..MAX Default N=1 
// POL is of type sc_port_policy
// POL defaults to SC_ONE_OR_MORE_BOUND
```

### POL

POL is of type `sc_port_policy` and it is an enumerated type and has three legal values:

+ SC_ONE_OR_MORE_BOUND
+ SC_ZERO_OR_MORE_BOUND
+ SC_ALL_BOUND

The value of POL enables different checking regarding the connectivity to the port.



## SystemC Exports



[^1]: D. C. Black, J. Donovan, B. Bunton, and A. Keist, *SystemC: From the Ground Up, Second Edition*, 2nd ed. Springer US, 2010.