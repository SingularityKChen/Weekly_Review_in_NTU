---
layout: post
title: "[Tutorial] Configure and Run PULPissimo"
description: "From install gcc and SDK to Run simple runtime examples for PULPissimo"
categories: [Tutorial]
tags: [Pulpissimo, Pulp]
last_updated: 2021-04-16 12:23:00 GMT+8
excerpt: "From install gcc and SDK to Run simple runtime examples for PULPissimo"
redirect_from:
  - /2020/12/20/
---

* Kramdown table of contents
{:toc .toc}
# Configure and Run PULPissimo

## Install Pulp GCC tool-chain and SDK

### Install GCC Tool-chain

```bash
sudo apt-get install autoconf automake autotools-dev curl libmpc-dev libmpfr-dev libgmp-dev gawk build-essential bison flex texinfo gperf libtool patchutils bc zlib1g-dev

git clone --recursive https://github.com/pulp-platform/pulp-riscv-gnu-toolchain
```

Then install the pulp:

```bash
cd pulp-riscv-gnu-toolchain

./configure --prefix=/opt/riscv --with-arch=rv32imc --with-cmodel=medlow --enable-multilib

make
```

If you meet this error, then maybe you need to use `sudo make` to build the tool-chain.

```
mkdir -p -- /opt/riscv /opt/riscv
mkdir: cannot create directory ‘/opt/riscv’: Permission denied
mkdir: cannot create directory ‘/opt/riscv’: Permission denied
```

You will see this info if you build it successfully.

```
make[9]: Nothing to be done for 'install-data-am'.
make[9]: Leaving directory '/home/singularity/pulp-riscv-gnu-toolchain/build-gcc-newlib-stage2/riscv32-unknown-elf/rv32imcxgap9/ilp32/libstdc++-v3'
make[8]: Leaving directory '/home/singularity/pulp-riscv-gnu-toolchain/build-gcc-newlib-stage2/riscv32-unknown-elf/rv32imcxgap9/ilp32/libstdc++-v3'
make[7]: Leaving directory '/home/singularity/pulp-riscv-gnu-toolchain/build-gcc-newlib-stage2/riscv32-unknown-elf/rv32imcxgap9/ilp32/libstdc++-v3'
make[6]: Leaving directory '/home/singularity/pulp-riscv-gnu-toolchain/build-gcc-newlib-stage2/riscv32-unknown-elf/libstdc++-v3'
make[5]: Nothing to be done for 'install-data-am'.
make[5]: Leaving directory '/home/singularity/pulp-riscv-gnu-toolchain/build-gcc-newlib-stage2/riscv32-unknown-elf/libstdc++-v3'
make[4]: Leaving directory '/home/singularity/pulp-riscv-gnu-toolchain/build-gcc-newlib-stage2/riscv32-unknown-elf/libstdc++-v3'
make[3]: Leaving directory '/home/singularity/pulp-riscv-gnu-toolchain/build-gcc-newlib-stage2/riscv32-unknown-elf/libstdc++-v3'
make[2]: Leaving directory '/home/singularity/pulp-riscv-gnu-toolchain/build-gcc-newlib-stage2'
make[1]: Leaving directory '/home/singularity/pulp-riscv-gnu-toolchain/build-gcc-newlib-stage2'
mkdir -p stamps/ && touch stamps/build-gcc-newlib-stage2
```

Then add the this path into the environment `PATH`:

```bash
export PATH=/opt/riscv:$PATH
```

Also you need to add a env variable:

```bash
export PULP_RISCV_GCC_TOOLCHAIN=/opt/riscv
```

### Install Pulp SDK

Firstly, check the packages:

```bash
sudo apt install git python3-pip python-pip gawk texinfo libgmp-dev libmpfr-dev libmpc-dev swig3.0 libjpeg-dev lsb-core doxygen python3-sphinx sox graphicsmagick-libmagick-dev-compat libsdl2-dev libswitch-perl libftdi1-dev cmake scons libsndfile1-dev

sudo pip3 install artifactory twisted prettytable sqlalchemy pyelftools openpyxl xlsxwriter pyyaml numpy configparser pyvcd

sudo pip2 install configparser
```

Then, the following environment variable must point to the folder where the platform was installed:

```bash
# export VSIM_PATH=<pulpissimo root folder>/sim
export VSIM_PATH=$HOME/pulpissimo/sim
```

After that, you can use the build in command to build the SDK from source at the root directory of Pulpissimo:

```bash
make build-pulp-sdk
```

You will see this or similar information if you build them successfully.

```
copying extra files... done
dumping search index in English (code: en) ... done
dumping object inventory... done
build succeeded, 7 warnings.

The HTML pages are in _build/html.

Build finished. The HTML pages are in _build/html.
make[2]: Leaving directory '/home/singularity/pulpissimo/pulp-sdk/doc/top'
make[1]: Leaving directory '/home/singularity/pulpissimo/pulp-sdk/doc'
Reached EOF with exit status 0
```

## Update IPs

Run this command at the root directory at Pulpissimo.

```bash
make checkout
```

It will download all the related submodules.

## Get the Runtime Test

### Clone the GitHub repository

```bash
make pulp-runtime
```

### Configure environment for PULPissimo

```bash
cd pulp-runtime
source configs/pulpissimo.sh
```

### Building the RTL simulation platform

```bash
source setup/vsim.sh
make build
```

If you meet is kind of error, you need to install ModelSim.

```
/bin/bash: vlib: command not found
```

If you successfully build it, you will see:

```
Optimized design name is vopt_tb
End time: 14:46:39 on Dec 20,2020, Elapsed time: 0:00:08
Errors: 0, Warnings: 22, Suppressed Warnings: 200
make[1]: Leaving directory '/home/singularity/pulpissimo/sim'
cp -r rtl/tb/* /home/singularity/pulpissimo/install
```

### Downloading and try runtime examples

```bash
git clone git@github.com:pulp-platform/pulp-rt-examples.git
```

#### Prepare the Environments

Before you begin to execute the runtime example, you need to export some system environment variables by:

```bash
# at the root directory of Pulpissimo
source ./pulp-runtime/configs/pulpissimo.sh

source ./env/pulpissimo.sh

source ./pulp-sdk/configs/pulpissimo.sh 
```

If it says:

```
Setting up SDK
bash: /home/singularity/pulpissimo/pulp-sdk/sourceme.sh: No such file or directory
Setting up for RTL simulation
Setting up VSIM
```

Then you need to rebuild the SKD and you'll get the `sourceme.sh`:

```bash
cd pulp-sdk
make all
```

Otherwise it will say:

```
Makefile:6: /install/rules/pulp_rt.mk: No such file or directory
make: *** No rule to make target '/install/rules/pulp_rt.mk'.  Stop
```

I recommend you to source the `./pulp-sdk/sourceme.sh` again after this.

#### Run `hellow`

Then Let's run `hellow`:

```bash
cd pulp-rt-examples/hellow
make all run
```

If you want to see the waveform, then,

```bash
cd pulp-rt-examples/hellow
make clean all
make run vsim/script=export_run.tcl
```

However, I found that it's quite slow to generate the `.vcd` file and open it using `gtkwave`.

Therefore, you can use the GUI to simulate and monitor the waveform.

### Run Simulations after first build

After you first build all the environment, you also need to source some files before you execute your simulation again.

#### Export environment variables (Optional)

You need to check whether you have the following environment variables for toolchain:

```bash
export PULP_RISCV_GCC_TOOLCHAIN=$RISCV
export PATH=$PATH:$RISCV/bin
export VSIM_PATH=$PULP/pulpissimo/sim
```

#### Select targets for this simulation (Important)

```bash
cd pulp-sdk
# Target select
source configs/pulpissimo.sh
# Platform select
source configs/platform-rtl.sh
# SDK setup
source pkg/sdk/dev/sourceme.sh

# RTL setup
cd pulpissimo
source setup/vsim.sh
```

#### Update RTL (Optional)

If you changed your RTL, you need to rebuild your RTL.

```bash
cd pulpissimo
./generate-scripts
make clean build
```

#### Compile and execute new C file

Just the same as in `Run hellow`.