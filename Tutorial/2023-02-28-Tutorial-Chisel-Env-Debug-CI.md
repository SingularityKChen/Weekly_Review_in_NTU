---
layout: post
title: "[Tutorial] Quick Debug and Run Test on Chisel Repos based on CI Flow Files"
description: "This tutorial introduces the quick way to debug the code of Chisel environment, such as Chisel3, playground, Rocket Chip,  et al. The method introduced in this tutorial can also be used for other repos."
categories: [Tutorial]
tags: [Chisel, CI, playgroud, Rocket Chip]
last_updated: 2023-02-28 10:41:00 GMT+8
excerpt: "This tutorial introduces the quick way to debug the code of Chisel environment, such as Chisel3, playground, Rocket Chip,  et al. The method introduced in this tutorial can also be used for other repos."
redirect_from:
  - /2023/02/28/
---

* Kramdown table of contents
{:toc .toc}

# Chisel Environment Quick Debug and Run Test based on CI Flow Files

This tutorial introduces the quick way to debug the code of Chisel environment, such as [Chisel3](https://github.com/chipsalliance/chisel3), [playground](https://github.com/chipsalliance/playground), [Rocket Chip](https://github.com/chipsalliance/rocket-chip), *et al*. The method introduced in this tutorial can also be used for other repos.

## Quick Start Method

The fastest way to debug the code in one repo is reading the CI file and dig the test command from them.

## [playground](https://github.com/chipsalliance/playground)

### Install Docker

Please try to search in your browser or ask ChatGPT how to install Docker in your OS.

### Run Test

Based on [the CI file](https://github.com/chipsalliance/playground/blob/c92d6127e537037cc8106cfe90cacc2a3889315f/.github/workflows/bump.yml), the test should be executed in Arch Linux.

```bash
git clone https://github.com/chipsalliance/playground.git
cd playground
# init and update all the repos
make init bump update-patches patch
cd ../
docker run -it --rm -v playground:/workspace/playground singularitykchen/playground:dev_latest --workdir /workspace/playground
```

In your Docker:
```bash
make compile test
```

## [Rocket Chip](https://github.com/chipsalliance/rocket-chip)

Based on [the CI file](https://github.com/chipsalliance/rocket-chip/blob/0e4af6df500287d7ea0e934d99197e162da69855/.github/workflows/mill-ci.yml),

There are three types of CI tests:
+ `emulator`
+ `riscv-tests`
+ `riscv-arch-test`

Only the first one don't need `RISCV` variable. For other two, you need to execute the following command based on [this CI file](https://github.com/chipsalliance/rocket-chip/blob/0e4af6df500287d7ea0e934d99197e162da69855/.github/workflows/continuous-integration.yml):
```bash
# install tools and set environment
make tools -C regression SUITE=none
```

Each test contains several sub-tasks with different configurations. 
I'd suggest executing one of the emulator test with config `DualBankConfig` firstly then others.

Below only list one config name as an example. For the full configurations, please try to find them in [the CI file](https://github.com/chipsalliance/rocket-chip/blob/0e4af6df500287d7ea0e934d99197e162da69855/.github/workflows/mill-ci.yml).

```bash
# emulator
mill -i "emulator[freechips.rocketchip.system.TestHarness,freechips.rocketchip.system.DualBankConfig].elf"
```

```bash
# install tools and set environment
make tools -C regression SUITE=none
# riscv-tests
mill -i -j 0 "runnable-test[freechips.rocketchip.system.TestHarness,freechips.rocketchip.system.TinyConfig,_,_].run"
# riscv-arch-test
mill -i -j 0 "runnable-arch-test[freechips.rocketchip.system.TestHarness,freechips.rocketchip.system.DefaultRV32Config,32,RV32IMACZicsr_Zifencei].run"
```

## [Chisel3](https://github.com/chipsalliance/chisel3)

This is also the same. 
Now it is your turn to read [this CI file](https://github.com/chipsalliance/chisel3/blob/9f416ddade4b7a007d74d83d0fefbf427f9e207b/.github/workflows/test.yml) and find the commands.