---
layout: post
title: "[CodeStudy] Undefined reference to one function in CPP"
description: "How to solve the error of Undefined reference to one function in CPP"
categories: [CodeStudy]
tags: [CPP]
last_updated: 2020-08-04 11:28:00 GMT+8
excerpt: "This blog shows a error caused by the separation of the declaration and implementation in template class."
redirect_from:
  - /2020/08/04/
---

* Kramdown table of contents
{:toc .toc}
# Undefined reference to one function in CPP

I met the error info in this code line `peUnitArray[unitCol]->peUnitMpy(&weightDataBV, &inActBV);`

```
/usr/bin/ld: CMakeFiles/Arch_v2.dir/pe_row_array.cpp.o: in function `PERowArray::peRowArrayTh()':
/home/singularity/Arch_v2/pe_row_array.cpp:91: undefined reference to `PE_Unit<4u, 3u, 3u>::peUnitMpy(sc_dt::sc_bv<32>*, sc_dt::sc_bv<96>*)'
```

The class `PE_Unit` is a template class, and the function `peUnitMpy` is implemented in `.cpp` file instead of `.h` file.

So after I move the implementation into the head file, it works.

```c++
// Before

// file.h
template<unsigned int A, unsigned int B, unsigned int C>
class PE_Unit{
    void peUnitMpy(XXXX);
}
// file.cpp
template<unsigned int A, unsigned int B, unsigned int C>
void PE_Unit<A, B, C>::peUnitMpy(XXXX){
    //implementations 
}
```

From [this](https://blog.csdn.net/u012814856/article/details/84645963) blog, I know that it's better to implementation the functions of template class in the `.h` files.

For detailed information, see the link above.