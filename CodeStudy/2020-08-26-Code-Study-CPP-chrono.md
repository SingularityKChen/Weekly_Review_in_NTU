---
layout: post
title: "[CodeStudy] Using C++ Lib Chrono to record execution time"
description: "Chrono is a flexible collection of types that track time with varying degrees of precision."
categories: [CodeStudy]
tags: [CPP, Chrono]
last_updated: 2020-08-26 14:31:00 GMT+8
excerpt: "Chrono is a flexible collection of types that track time with varying degrees of precision."
redirect_from:
  - /2020/08/26/
---

* Kramdown table of contents
{:toc .toc}
# Using C++ Lib Chrono to record execution time

Chrono is a flexible collection of types that track time with varying degrees of precision.

Here are several methods that can record the execution time.

## `ctime`

```c++
#include <ctime>
using namespace std;

clock_t start = clock();
// do something...
clock_t end   = clock();
cout << "cost" << (double)(end - start) / CLOCKS_PER_SEC << "s" << endl;
```

## `chrono`

```c++
#include <chrono>
auto start = steady_clock::now();
auto end = steady_clock::now();
auto duration = duration_cast<milliseconds>(end - start);
printf("[INFO] Execution time %ld ms\n", duration.count());
```

You can change the unit of the time with `duration_cast`, including:

+ nanoseconds
+ microseconds
+ milliseconds
+ seconds
+ minutes
+ hours