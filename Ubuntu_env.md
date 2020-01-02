# Some lib

## one_line

```shell
cd ~
sudo apt-get install vim git autoconf automake autotools-dev curl device-tree-compiler libmpc-dev libmpfr-dev libgmp-dev gawk build-essential bison flex texinfo gperf libtool patchutils bc zlib1g-dev iverilog gtkwave verilator yosys scala python-pip

sudo pip install jupyter notebook
```

## sbt
```shell
echo "deb https://dl.bintray.com/sbt/debian /" | sudo tee -a /etc/apt/sources.list.d/sbt.list
curl -sL "https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x2EE0EA64E40A89B84B2DF73499E82A75642AC823" | sudo apt-key add
sudo apt-get update
sudo apt-get install sbt
```

# Some gits

## firrtl

```shell
cd ~
sudo git clone https://github.com/freechipsproject/firrtl.git && cd firrtl
sudo sbt compile
sudo sbt test
sudo sbt assembly
sudo sbt publishLocal
```



## Chisel3

```shell
cd ~
sudo git clone https://github.com/freechipsproject/chisel3.git && cd chisel3
sudo sbt compile
sudo sbt test
sudo sbt publishLocal
```

