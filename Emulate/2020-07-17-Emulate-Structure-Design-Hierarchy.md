---
layout: post
title: "[Emulate] Structure Design Hierarchy"
description: "Structure Design Hierarchy, chapter 10 of the book From the Ground Up."
categories: [Emulate]
tags: [SystemC]
last_updated: 2020-07-17 17:50:00 GMT+8
excerpt: "This post introduces six approaches of design hierarchy. It's the chapter 10 of the book From the Ground Up."
redirect_from:
  - /2020/07/17/
---

* Kramdown table of contents
{:toc .toc}
# Structure Design Hierarchy[^1]

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200717171034.png" alt="Design Hierarchy" style="zoom:50%;" />

## Module Hierarchy

C++ offers two basic ways to create submodule objects within the definition of a parent module or object. First, a submodule object may **be created directly by declaration** just like any simple member data of the module class. Second, a submodule object may **be indirectly referenced by means of a pointer** in combination with dynamic allocation.

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200717171546.png" alt="Comparison of hierarchical instantiation approaches" style="zoom:50%;" />

+ Direct top-level ( `sc_main`)
+ Indirect top-level ( `sc_main`)
+ Direct submodule header-only
+ Direct submodule
+ Indirect submodule header-only
+ Indirect submodule

## Direct Top-Level Implementation

```c++
//FILE: main.cpp 
#include <systemc> 
#include "Car.h" 
int sc_main(int argc, char* argv[]) { 
    Car car_i("car_i"); 
    sc_start(); 
    return 0;
}
```

## Indirect Top-Level Implementation

It adds the possibility of dynamically configuring the design with the addition of if-else and looping constructs.

```c++
//FILE: main.cpp 
#include <systemc> 
#include "Car.h" 
intsc_main(int argc, char* argv[]) { 
    Car* car_iptr;
    // pointer to Car
    car_iptr = new Car("car_i"); 
    // create Car 
    sc_start(); 
    delete car_iptr; 
    return 0;
}
```

## Direct Submodule Header-Only Implementation

```c++
//FILE:Car.h 
#include "Body.h" 
#include "Engine.h" 
SC_MODULE(Car) { 
    Body body_i;
    Engine eng_i ; 
    SC_CTOR(Car)
        : body_i("body_i") //initialization 
        , eng_i("eng_i") {
            // other initialization 
        }
};
```

## Direct Submodule Implementation

```c++
//FILE:Car.h 
#include "Body.h" 
#include "Engine.h" 
SC_MODULE(Car) { 
    Body body_i;
    Engine eng_i; 
    Car(sc_module_name nm);
};
```

```c++
//FILE:Car.cpp 
#include <systemc> 
#include "Car.h" // Constructor 
SC_HAS_PROCESS(Car); 
Car::Car(sc_module_name nm) 
    : sc_module(nm) 
    , body_i("body_i") 
    , eng_i("eng_i") {
        // other initialization
    }
```

## Indirect Submodule Header-Only Implementation

```c++
//FILE:Body.h 
#include "Wheel.h" 
SC_MODULE(Body) { 
    Wheel* wheel_FL_iptr; 
    Wheel* wheel_FR_iptr; 
    SC_CTOR(Body) { 
        wheel_FL_iptr = new Wheel("wheel_FL_i"); 
        wheel_FR_iptr = new Wheel("wheel_FR_i"); 
        // other initialization
    }
};
```

## Indirect Submodule Implementation

Moving the module indirect approach into the implementation file has the advantage of possibly supplying pre-compiled object files making this approach good for IP distribution. 

This advantage is in addition to the possibility of dynamically determining the configuration discussed previously.

This approach is good for both internal and external IP distribution.

```c++
//FILE:Engine.h 
class FuelMix; 
class Exhaust; 
class Cylinder; 
SC_MODULE(Engine) { 
    FuelMix* fuelmix_iptr; 
    Exhaust* exhaust_iptr; 
    Cylinder* cyl1_iptr; 
    Cylinder* cyl2_iptr; 
    Engine(sc_module_name nm); // Constructor
};
```

```c++
//FILE: Engine.cpp 
#include <systemc> 
#include "FuelMix.h" 
#include "Exhaust.h" 
#include "Cylinder.h" 
// Constructor 
SC_HAS_PROCESS(Engine); 
Engine::Engine(sc_module_name nm) 
: sc_module(nm) 
{
    fuelmix_iptr = new FuelMix("fuelmix_i"); 
    exhaust_iptr = new Exhaust("exhaust_i"); 
    cyl1_iptr = new Cylinder("cyl1_i"); 
    cyl2_iptr = new Cylinder("cyl2_i");
    // other initialization
}
```



[^1]: D. C. Black, J. Donovan, B. Bunton, and A. Keist, *SystemC: From the Ground Up, Second Edition*, 2nd ed. Springer US, 2010.