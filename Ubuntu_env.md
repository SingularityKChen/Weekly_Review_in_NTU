# Some Lib

## Some Dependence

```shell
cd ~
sudo apt-get install vim git autoconf automake autotools-dev curl device-tree-compiler libmpc-dev libmpfr-dev libgmp-dev gawk build-essential bison flex texinfo gperf libtool patchutils bc zlib1g-dev iverilog gtkwave verilator yosys scala python-pip

sudo pip install jupyter notebook
```

## Sbt
```shell
echo "deb https://dl.bintray.com/sbt/debian /" | sudo tee -a /etc/apt/sources.list.d/sbt.list
curl -sL "https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x2EE0EA64E40A89B84B2DF73499E82A75642AC823" | sudo apt-key add
sudo apt-get update
sudo apt-get install sbt
```

## Install Jave 8 and Set Default Version
```shell
sudo apt install openjdk-8-jdk-headless
#optional
sudo update-alternatives --config java
sudo update-alternatives --config javac
```

## Scala Kernel for Jupyter (optional)
```shell
curl -L -o coursier https://git.io/coursier && chmod +x coursier
SCALA_VERSION=2.12.8 ALMOND_VERSION=0.2.1
./coursier bootstrap -r jitpack -i user -I user:sh.almond:scala-kernel-api_$SCALA_VERSION:$ALMOND_VERSION sh.almond:scala-kernel_$SCALA_VERSION:$ALMOND_VERSION --sources --default=true -o almond
./almond --install
```

# Some Gits (after sbt)

## Chipyard

### QEMU Dependence

Error When installing the RISC-V tools:

```shell
==>  Configuring qemu

ERROR: glib-2.48 gthread-2.0 is required to compile QEMU

ERROR: pixman >= 0.21.8 not present.
       Please install the pixman devel package.
```

So we need to install the lib before installing the RISC-V tools:

```shell
sudo apt-get install libglib2.0-dev libpixman-1-dev
```

### Setup
```shell
git clone https://github.com/ucb-bar/chipyard.git
cd chipyard
./scripts/init-submodules-no-riscv-tools.sh
```

### Installing the RISC-V Tools

The shell needs `bash` or you have to correct some commands in `env.sh`
```shell
./scripts/build-toolchains.sh
source ./env.sh
```

## Firrtl(no need if git chipyard)

```shell
cd ~
sudo git clone https://github.com/freechipsproject/firrtl.git && cd firrtl
sudo sbt compile
sudo sbt test
sudo sbt assembly
sudo sbt publishLocal
```


## Chisel3(no need if git chipyard)

```shell
cd ~
sudo git clone https://github.com/freechipsproject/chisel3.git && cd chisel3
sudo sbt compile
sudo sbt test
sudo sbt publishLocal
```



## Bootcamp (optional)

```shell
git clone https://github.com/freechipsproject/chisel-bootcamp.git
cd chisel-bootcamp
mkdir -p ~/.jupyter/custom
cp source/custom.js ~/.jupyter/custom/custom.js
```
