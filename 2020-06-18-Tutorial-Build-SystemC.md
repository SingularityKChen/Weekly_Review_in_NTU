---
layout: post
title: "[Tutorial] Build SystemC Environment"
description: "How to Build SystemC Environment in Windows and Linux Ubuntu."
categories: [Tutorial]
tags: [SystemC]
last_updated: 2020-06-23 22:02:00 GMT+8
excerpt: "How to Build SystemC Environment in Windows and Linux Ubuntu."
redirect_from:
  - /2020/06/18/
---

* Kramdown table of contents
{:toc .toc}
# Build SystemC Environment

## In Windows

I used` CLion` as the IDE tool and `MinGW` as the Compatibility layer. You can also choose Visual Studio as the IDE tool, which will be more convenient in the environment setup.

### Visual Studio

You can find the installation notes in the file `./INSTALL`.

```
The download directory contains two subdirectories: 'msvc10' and
'examples/build-msvc'.

The 'msvc10' directory contains the project and workspace files to
compile the 'systemc.lib' library. Double-click on the 'SystemC.sln'
file to launch Visual C++ 2010 with the workspace file. The workspace file
will have the proper switches set to compile for Visual C++ 2010.
Select `Build SystemC' under the Build menu or press F7 to build
`systemc.lib'.

The `examples/build-msvc' directory contains the project and workspace
files to compile the SystemC examples. Go to one of the examples
subdirectories and double-click on the .vcxproj file to launch Visual C++
with the workspace file. The workspace file will have the proper switches
set to compile for Visual C++ 2010. Select 'Build <example>.exe' under the
Build menu or press F7 to build the example executable.

For convenience, a combined solution file 'SystemC_examples.sln' with
all example projects can be found in the 'examples/build-msvc' directory.
A similar solution file 'tlm_examples.sln' for the TLM examples is available
as well.

The provided project files are prepared for both the 32-bit 'Win32' and
64-bit 'x64' configurations.  Please refer to the Microsoft Visual Studio
documentation for details about 64-bit builds.

In addition to building static libraries for SystemC, the provided project
files include support for building a SystemC DLL (configurations DebugDLL,
ReleaseDLL).
```

### CLion

In some blogs, e.g., [this one](https://www.programmersought.com/article/3712770251/), said that we should compile it under `./src/` and needed to change the `./scr/CMakeLists.txt`, which is quite complex I thought.

Actually, we can simple open the whole root directory, then it will be automatically compiled with this info:

```
[100%] Built target systemc

Build finished
```

Or you can build it with <kbd>ctrl</kbd> + <kbd>F9</kbd>.

Then, under `Build` tool bar of CLion, you can find the `install` option. Then click it, and you can successively install the `SystemC`.

If you meet the error that you don't have the permission to access the directories, then you need to change the installation directory defined in `./cmake-build-degug/cmake_install.cmake` file.

```
# Set the install prefix
if(NOT DEFINED CMAKE_INSTALL_PREFIX)
  set(CMAKE_INSTALL_PREFIX "C:/Program Files/SystemC")
endif()
```

Just replace the `CMAKE_INSTALL_PREFIX` to another directory that you have permission to access.

### Errors: `sc_core`

If you meet the error:

```
CMakeFiles\Adder.dir/objects.a(main.cpp.obj): In function `__static_initialization_and_destruction_0':
E:/systemc-2.3.3/SystemC/include/sysc/kernel/sc_ver.h:182: undefined reference to `sc_core::sc_api_version_2_3_3_cxx201402L<&sc_core::SC_DISABLE_VIRTUAL_BIND_UNDEFINED_>::sc_api_version_2_3_3_cxx201402L(sc_core::sc_writer_policy)'
collect2.exe: error: ld returned 1 exit status
```

Then you can change the `CMAKE_CXX_STANDARD` from 98 to 14.[^1]

```
set (CMAKE_CXX_STANDARD 98 CACHE STRING
     "C++ standard to build all targets. Supported values are 98, 11, and 14.")
     
set (CMAKE_CXX_STANDARD 14)
```

And then `rebuild` the project, install it again.

## Ubuntu 20.04 LTS

Added on 23 June, 2020.

------

I followed the instructions of [this video](https://www.youtube.com/watch?v=rLBScPm_bis) and set up the environment for Ubuntu 20.04 LTS.

You need to install these firstly: `cmake`, `g++`, and `doxygen`.

```shell
# check the softwares
g++ -v
cmake --version
doxygen
# if you don't have, then you need to `apt-get install xxx`
# enter the directory you download the file
cd systemc-2.3.3/
# create a directory to generate some temp files
mkdir buildDebug
cd buildDebug/
# -DCMAKE_INSTALL_PREFIX is the directory you install SystemC
# be care of -DCMAKE_CXX_STANDARD
cmake -DCMAKE_BUILD_TYPE=Debug -DCMAKE_DEBUG_POSTFIX="-d" -DCMAKE_INSTALL_PREFIX=$HOME/systemc-2.3.3-install -DCMAKE_CXX_STANDARD=14 -DBUILD_SOURCE_DOCUMENTATION=ON ..
# -j means the number of jobs run parallelly
make -j 6
make check -j 6
make install
```

Then you can find `include`, `lib` file folders under that directory.

# Example `CMakeLists`

```
cmake_minimum_required(VERSION 3.16)
project(Adder)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_PREFIX_PATH E:/systemc-2.3.3/SystemC)

include_directories(${CMAKE_PREFIX_PATH}/include)
find_package(SystemCLanguage CONFIG REQUIRED)
link_directories(${CMAKE_PREFIX_PATH}/lib)
add_executable(Adder main.cpp)
target_link_libraries(Adder SystemC::systemc)
```

## `CMAKE_CXX_STANDARD` 

The `CMAKE_CXX_STANDARD` should be the same as that in compiling stage.

## `include_directories`

The directory of SystemC include files you installed in.

## `link_directories`

The directory of SystemC library you installed in.

## `add_executable`

All the C++ files of your project.

## `find_package` and `target_link_libraries`

Will cause errors like following if absent.

```
CMakeFiles\Adder.dir/objects.a(main.cpp.obj): In function `Hello_SystemC::method1()':
F:/main.cpp:18: undefined reference to `sc_core::sc_time_stamp()'
CMakeFiles\Adder.dir/objects.a(main.cpp.obj): In function `Hello_SystemC::method2()':
F:/main.cpp:24: undefined reference to `sc_core::sc_time_stamp()'
CMakeFiles\Adder.dir/objects.a(main.cpp.obj): In function `sc_main':
F:/main.cpp:30: undefined reference to `sc_core::sc_time::sc_time(double, sc_core::sc_time_unit)'
F:/main.cpp:31: undefined reference to `sc_core::sc_clock::sc_clock(char const*, sc_core::sc_time const&, double, sc_core::sc_time const&, bool)'
F:/main.cpp:32: undefined reference to `sc_core::sc_module_name::sc_module_name(char const*)'
F:/main.cpp:32: undefined reference to `sc_core::sc_module_name::~sc_module_name()'
F:/main.cpp:31: undefined reference to `sc_core::sc_clock::~sc_clock()'
F:/main.cpp:35: undefined reference to `sc_core::sc_module_name::~sc_module_name()'
F:/main.cpp:31: undefined reference to `sc_core::sc_clock::~sc_clock()'
CMakeFiles\Adder.dir/objects.a(main.cpp.obj): In function `__static_initialization_and_destruction_0':
```



[^1]: https://stackoverflow.com/questions/46875731/setting-up-a-systemc-project-with-cmake-undefined-reference-to-sc-core