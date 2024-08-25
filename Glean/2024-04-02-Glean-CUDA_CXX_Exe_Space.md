---
layout: post
title: "[Glean] CUDA C++ Function Execution Space"
description: "CUDA C++ Function Execution Space, including `__global__`, `__device__`, `__host__`, and `__host__ __device__`. A summary table is provided for comparison at the end."
categories: [Glean]
tags: [CUDA, NVIDIA]
last_updated: 2024-06-27 11:17:00 GMT+8
excerpt: "CUDA C++ Function Execution Space, including `__global__`, `__device__`, `__host__`, and `__host__ __device__`. A summary table is provided for comparison at the end."
redirect_from:
  - /2024/04/02/
---

* Kramdown table of contents
{:toc .toc}

# CUDA C++ Function Execution Space[^1]

## `__global__`

The `__global__` execution space specifier declares a function as being a kernel. Such a function is:

- Executed on the device,
- Callable from the host,
- Callable from the device for devices of compute capability 5.0 or higher.

## `__device__`

The `__device__` execution space specifier declares a function that is:

- Executed on the device,
- Callable from the device only.

## `__host__`

The `__host__` execution space specifier declares a function that is:

- Executed on the host,
- Callable from the host only.

## `__host__ __device__`

The `__host__ __device__` execution space specifier declares a function that is:

- Executed on the host or the device,
- Callable from the host or the device.

# Example

```cpp
#include <iostream>

__global__ void kernel() {
    printf("Hello from the device!\n");
}

__host__ void host_function() {
    printf("Hello from the host!\n");
}

__device__ void device_function() {
    printf("Hello from the device!\n");
}

__host__ __device__ void host_device_function() {
    printf("Hello from the host or the device!\n");
}

int main() {
    host_function(); // Call the host function
    kernel<<<1, 1>>>(); // Launch the kernel on the device
    cudaDeviceSynchronize(); // Wait for the kernel to finish
    device_function<<<1, 1>>>(); // Launch the device function
    cudaDeviceSynchronize(); // Wait for the device function to finish
    host_device_function(); // Call the host-device function
    return 0;
}
```

# Summary

We summarize the function execution spaces in CUDA C++ in the following table:

| Execution Space | Description | Callable from Host | Callable from Device | Executed on Host | Executed on Device |
| :---: | :---: | :---: | :---: | :---: | :---: |
| `__global__` | Kernel | Yes | Yes | No | Yes |
| `__device__` | Device | No | Yes | No | Yes |
| `__host__` | Host | Yes | No | Yes | No |
| `__host__ __device__` | Host or Device | Yes | Yes | Yes | Yes |

# References

[^1]: [CUDA C++ Programming Guide 12.4](https://docs.nvidia.com/cuda/cuda-c-programming-guide/#function-execution-space-specifiers)
