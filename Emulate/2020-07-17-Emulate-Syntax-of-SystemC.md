---
layout: post
title: "[Emulate] Syntaxes of SystemC"
description: "Syntaxes of SystemC, including the different syntaxes of SystemC module and constructors."
categories: [Emulate]
tags: [SystemC]
last_updated: 2020-07-17 11:45:00 GMT+8
excerpt: "This blog introduces the different syntaxes of SystemC module and constructors."
redirect_from:
  - /2020/07/17/
---

* Kramdown table of contents
{:toc .toc}
# Syntaxes of SystemC[^1]

## `SC_MODULE`

> Within this derived module class, a variety of elements make up the MODULE BODY: 
>
> + Ports
> + Member channel instances
> + Member data instances
> + Member module instances (sub-designs)
> + Constructor
> + Destructor
> + Simulation process member functions (processes)
> + Other methods (i.e., member functions)

### The Macro Style

```c++
#include <systemc> 
SC_MODULE(module_name) {
    MODULE_BODY
};
```

### C++ Style

```c++
#include <systemc> 
class module_name : public sc_module { 
    public:
    MODULE_BODY
};
```

## Constructors

### `SC_CTOR`

```c++
//FILE: basic_process_ex.h 
SC_MODULE(basic_process_ex) { 
    SC_CTOR(basic_process_ex) { 
        SC_THREAD(my_thread_process);
    } 
    void my_thread_process(void);
};
```

```c++
//FILE: basic_process_ex.cpp 
void basic_process_ex::my_thread_process(void) { 
    cout << "my_thread_process executed within " << name() 
        //returns sc_module instance name
        << endl;
}
```



### `SC_HAS_PROCESS`

> You can use this macro in two situations. 
>
> First, use `SC_HAS_PROCESS` when you require constructors **with arguments** beyond just the SystemC module instance name string passed into `SC_CTOR` (e.g., to provide configurable modules). 
>
> Second, use `SC_HAS_PROCESS` when you want to **place the constructor in the implementation** (i.e., .cpp) file.

#### Syntax of `SC_HAS_PROCESS` in the header

```c++
//FILE: module_name.h 
SC_MODULE(module_name) { 
    SC_HAS_PROCESS(module_name); 
    module_name(sc_module_name instname[, other_args…])
        : sc_module(instname) [, other_initializers] {
            CONSTRUCTOR_BODY 
        }
};
```



#### Syntax of `SC_HAS_PROCESS` separated

```c++
//FILE: module_name.h 
SC_MODULE(module_name) { 
    module_name(sc_module_name
                instname[,other_args…]);
};
```

```c++
//FILE: module_name.cpp 
SC_HAS_PROCESS(module_name); 
module_name::module_name(
    sc_module_name instname[, other_args…])
    : sc_module(instname) [, other_initializers] 
    {
        CONSTRUCTOR_BODY
    }
```

## Two Styles Using SystemC Macros

### The Traditional Coding Style

```c++
// Traditional style NAME.h
#ifndef NAME_H 
#define NAME_H 
#include "submodule.h" …
SC_MODULE(NAME) { 
    Port declarations
        Channel/submodule instances 
        SC_CTOR(NAME) 
        : Initializations {
            Connectivity 
                Process registrations 
        }
    Process declarations 
        Helper declarations
};
#endif
```

```c++
// Traditional style NAME.cpp
#include <systemc> 
#include "NAME.h"
NAME::Process {implementations }
NAME::Helper {implementations }
```



### Recommended Alternate Style

```c++
// Recommended style NAME.h
#ifndef NAME_H 
#define NAME_H
Submodule forward class declarations 
SC_MODULE(NAME){ 
    Port declarations 
        Channel/Submodule* definitions 
        // Constructor declaration: 
        SC_CTOR(NAME); 
        Process declarations 
        Helper declarations
};
#endif
```

```c++
// Recommended style NAME.cpp
#include <systemc> 
#include "NAME.h" 
SC_HAS_PROCESS(NAME); 
NAME::NAME(sc_module_name nm)
: sc_module(nm) 
, Initializations {
    Channel allocations 
        Submodule allocations 
        Connectivity
        Process registrations 
}
NAME::Process {implementations }
NAME::Helper {implementations }
```



[^1]: D. C. Black, J. Donovan, B. Bunton, and A. Keist, *SystemC: From the Ground Up, Second Edition*, 2nd ed. Springer US, 2010.