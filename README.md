# Table of Contents

<!-- MarkdownTOC -->

1. [2019/12/16-22](#20191216-22)
	1. [Functional Programming Principles in Scala](#functional-programming-principles-in-scala)
		1. [Week1](#week1)
		1. [Week2](#week2)
	1. [Efficient Processing of Deep Neural Network: from Algorithms to Hardware Architectures](#efficient-processing-of-deep-neural-network-from-algorithms-to-hardware-architectures)
1. [2019/12/09-15](#20191209-15)
	1. [Git Command](#git-command)
		1. [the work flow](#the-work-flow)
		1. [Build a git project](#build-a-git-project)
	1. [Matrix Sum Accelerator](#matrix-sum-accelerator)
	1. [Chisel3 Syntax](#chisel3-syntax)
	1. [\(Linux\) C Syntax](#linux-c-syntax)
		1. [`asm volatile`](#asm-volatile)
		1. [create a C matrix with 2-D Array](#create-a-c-matrix-with-2-d-array)
		1. [Pointer and address access](#pointer-and-address-access)
	1. [RISC-V SPEC](#risc-v-spec)
	1. [Chipyard](#chipyard)
		1. [`SHA3`](#sha3)
		1. [`Hwacha`](#hwacha)
	1. [`RoCC` and extension instruction](#rocc-and-extension-instruction)
		1. [`RoCC` component](#rocc-component)
		1. [extension instruction with `RoCC`](#extension-instruction-with-rocc)
		1. [`HellaCache`](#hellacache)
	1. [Chisel-Tester2](#chisel-tester2)
	1. [Hammer](#hammer)
	1. [Computer Architecture](#computer-architecture)
		1. [Memory Fence and Memory Barrier](#memory-fence-and-memory-barrier)
		1. [Page Table Walk](#page-table-walk)
		1. [Translation Lookaside Buffer](#translation-lookaside-buffer)
		1. [Cache Hierarchy](#cache-hierarchy)
		1. [Memory Read \(Synchronous and Asynchronous\)](#memory-read-synchronous-and-asynchronous)
	1. [Some Trouble](#some-trouble)
		1. [`psutil`](#psutil)
		1. [Import classes into `IntelliJ IDEA`](#import-classes-into-intellij-idea)
		1. [Import reliance into `sbt` project](#import-reliance-into-sbt-project)
		1. [Can not find the lvy local lib](#can-not-find-the-lvy-local-lib)

<!-- /MarkdownTOC -->


# 2019/12/16-22

Last week, I cost Monday to review the growth in last two weeks, and Tuesday to prepare for the presentation in Thursday. And also, I improved the program of matrix sum I wrote two weeks ago, with an additional C test file. I learned the basic Scala knowledge on Wednesday but still lack some practice. Thursday to Saturday, I followed Shien's advice to watch the tutorial provided by one team in MIT who creates the Eyeriss project. A module of one translation project related to Chisel, named `Bootcamp`, also was done by me.

Unfortunately, due to the complex and chaos steps of both Rocket Core and FPGA, which need to boot Linux, I haven't do the hardware simulation. However, after the monthly report to my advisor in UESTC, I got a more simple method to skip the boot, I'd try after Shiqing came back.

So this week, I'll focus on some papers mentioned in the tutorial, including  bibliography 1-4, and several papers Shien suggested me to read, including bibliography  5-13.

\[1\] V. Sze, Y.-H. Chen, T.-J. Yang, and J. S. Emer, “Efficient Processing of Deep Neural Networks: A Tutorial and Survey,” *Proceedings of the IEEE*, vol. 105, no. 12, pp. 2295–2329, Dec. 2017, doi: [10.1109/JPROC.2017.2761740](https://doi.org/10.1109/JPROC.2017.2761740).

\[2\] Y.-H. Chen, T.-J. Yang, J. Emer, and V. Sze, “Eyeriss v2: A Flexible Accelerator for Emerging Deep Neural Networks on Mobile Devices,” *arXiv:1807.07928 [cs]*, May 2019.

\[3\] Y.-H. Chen, J. Emer, V. Sze, Y.-H. Chen, J. Emer, and V. Sze, “Eyeriss: a spatial architecture for energy-efficient dataflow for convolutional neural networks,” *ACM SIGARCH Computer Architecture News*, vol. 44, no. 3, pp. 367–379, Jun. 2016, doi: [10.1109/ISCA.2016.40](https://doi.org/10.1109/ISCA.2016.40).

\[4\] V. Sze, Y.-H. Chen, J. Emer, A. Suleiman, and Z. Zhang, “Hardware for Machine Learning: Challenges and Opportunities,” *2017 IEEE Custom Integrated Circuits Conference (CICC)*, pp. 1–8, Apr. 2017, doi: [10.1109/CICC.2017.7993626](https://doi.org/10.1109/CICC.2017.7993626).

\[5\] Y. Guo, “A Survey on Methods and Theories of Quantized Neural Networks,” *arXiv:1808.04752 [cs, stat]*, Dec. 2018.

\[6\] S. Zheng, Y. Liu, S. Yin, L. Liu, and S. Wei, “An efficient kernel transformation architecture for binary- and ternary-weight neural network inference,” presented at the Proceedings of the 55th Annual Design Automation Conference, 2018, p. 137, doi: [10.1145/3195970.3195988](https://doi.org/10.1145/3195970.3195988).

\[7\] Y. Hu *et al.*, “BitFlow: Exploiting Vector Parallelism for Binary Neural Networks on CPU,” in *2018 IEEE International Parallel and Distributed Processing Symposium (IPDPS)*, 2018, pp. 244–253, doi: [10.1109/IPDPS.2018.00034](https://doi.org/10.1109/IPDPS.2018.00034).

\[8\] Y. Chen *et al.*, “DaDianNao: A Machine-Learning Supercomputer,” in *2014 47th Annual IEEE/ACM International Symposium on Microarchitecture*, Cambridge, United Kingdom, 2014, pp. 609–622, doi: [10.1109/MICRO.2014.58](https://doi.org/10.1109/MICRO.2014.58).

\[9\] Y. Chen, T. Chen, Z. Xu, N. Sun, and O. Temam, “DianNao family: energy-efficient hardware accelerators for machine learning,” *Commun. ACM*, vol. 59, no. 11, pp. 105–112, Oct. 2016, doi: [10.1145/2996864](https://doi.org/10.1145/2996864).

\[10\] T. Chen *et al.*, “DianNao: a small-footprint high-throughput accelerator for ubiquitous machine-learning,” in *Proceedings of the 19th international conference on Architectural support for programming languages and operating systems - ASPLOS ’14*, Salt Lake City, Utah, USA, 2014, pp. 269–284, doi: [10.1145/2541940.2541967](https://doi.org/10.1145/2541940.2541967).

\[11\] S. Han *et al.*, “EIE: Efficient Inference Engine on Compressed Deep Neural Network,” *arXiv:1602.01528 [cs]*, May 2016.

\[12\] N. P. Jouppi *et al.*, “In-Datacenter Performance Analysis of a Tensor Processing UnitTM,” p. 17.

\[13\] Z. Du *et al.*, “ShiDianNao: shifting vision processing closer to the sensor,” in *Proceedings of the 42nd Annual International Symposium on Computer Architecture - ISCA ’15*, Portland, Oregon, 2015, pp. 92–104, doi: [10.1145/2749469.2750389](https://doi.org/10.1145/2749469.2750389).

## Functional Programming Principles in Scala

### Week1

#### Classes, Traits, Objects and Packages

##### Classes

Like in Java, classes can be instantiated using the new construct, there can be many “instances” (or “objects”) of the same class. Classes in Scala cannot have static members. You can use objects (see  below) to achieve similar functionality as with static members in Java.

##### Traits

Traits are like interfaces in Java, but they can also contain concrete  members, i.e. method implementations or field definitions.

##### Objects

Object in Scala are like classes, but for every object definition there is only one single instance.

##### Packages

Adding a statement such as package `foo.bar` at the top of a file makes the code in a file part of the package `foo.bar`. If you define a class `MyClass` in package `foo.bar`, you can import that  specific class (and not anything else from that package) with import  `foo.bar.MyClass`.

##### Source Files

Package hierarchies should be reflected in directory structure: a source file defining class C in package `foo.bar` should be stored in a  subdirectory as `foo/bar/C.scala`. Scala does not really enforce this  convention, but some tools such as the Scala IDE for eclipse might have  problems otherwise.

![coursera scala programming_and_computer](https://images-cdn.shimo.im/ityNPwFlFlYFAd2Y/coursera_scala_programming_and_computer.png)

![coursera_programming_theory](https://images-cdn.shimo.im/yMVPdcKxLiAcjY6p/coursera_programming_theory.png)

#### Coursera Elements of Programming

![coursera_elements_of_programming](./picture/coursera_elements_of_programming.png)

In Scala, usually `call-by-value`, but can use `=>` to use `call-by-name`.

If `call-by-value`, then it will calculate the values of the arguments firstly.

#### Blocks

`{}`

The definitions inside a block are only visible from within the block

It's not just name space control but it's also reusing outer definitions without passing them explicitly in parameters. 

#### Recursion

```scala
def factorial(n: Int): Int = 
	if (n == 0) 1 else n * factorial(n-1)
```

### Week2

#### High Order Functions

##### Function Type

```scala
def sum(f: Int => Int, a: Int, b: Int): Int =
	if (a > b) 0
	else f(a) + sum(f, a + 1, b)
def cube(x: Int): Int = x*x*x
def id(x:Int): Int = x
def sumInts(a: Int, b: Int) = sum(id, a, b)
def sumCubes(a: Int, b: Int) = sum(cube, a, b)
```

##### Anonymous Function

```scala
(x: Int) => x * x * x
(x: Int, y: Int) => x + y
```

```scala
def sum(f: Int => Int, a: Int, b: Int): Int =
	if (a > b) 0
	else f(a) + sum(f, a + 1, b)
def sumInts(a: Int, b: Int) = sum(x = > x, a, b)
def sumCubes(a: Int, b: Int) = sum(x => x * x * x, a, b)
```

```scala
def sum(f: Int => Int)(a: Int, b: Int): Int =
	if (a>b) 0
	else f(a) + sum(f)(a + 1, b)
def sumInts = sum(x => x)
def sumCubes = sum(x => x * x * x)
sumCubes(1, 10) + sumInts(10, 20)
```

#### Require and Assert

```scala
val x = sqrt(y)
assert(x >= 0)

class Balabala(x: Int, y: Int){
		require(y != 0, "y must be nonzero")
}
```

## Efficient Processing of Deep Neural Network: from Algorithms to Hardware Architectures

[2016 MIT](http://eyeriss.mit.edu/tutorial.html)

[2019 MIT](https://www.rle.mit.edu/eems/publications/tutorials/)

[slides and video](https://slideslive.com/38921492/efficient-processing-of-deep-neural-network-from-algorithms-to-hardware-architectures)

OneNote

# 2019/12/09-15

Last week, I finished the draft version of matrix sum with Chisel and became half-familiar with it. Also, I tried to write a functional test with `Chisel-tester2`. Unfortunately, due to the complex and unstable environment, I can not check all the syntax or run the functional test, but I tried to use the environment in `Jupyter` to run a simple test and to small submodules had been proved right. In order to use Chisel and `Chipyard`'s components, I studied one sample project named [`SHA3`](#SHA3). If I have more time, I'd back here to restudy that.

Fortunately, while I was programming the matrix sum project, I realized the feeling of distinguishing the difference of Scala types and hardware types. I mean, I knew why we can utilize the features of Scala to boost our efficiency of design our circuits and when we should create a hardware register to store our mediate data, why the hardware type `UInt` can not be translated to the Scala type `Int`. Just because Scala is the hardware generator, it's a software programming language, its types exist when we build the project. But as for hardware types, they only exist when the circuits running. So we can not get an unknown value and translate it to a must-know value.

So this week, I'll try to finished the related C files and run the stimulation on FPGA boards. Also, I need to learn some Scala programming skills to enhance my Chisel coding ability.

## Git Command
[some useful command and the explanation of git](https://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html).
![the work flow](https://www.ruanyifeng.com/blogimg/asset/2015/bg2015120901.png)

### the work flow

+ Workspace

+ Index/Stage

+ Repository

+ Remote

### Build a git project

#### Create one new project

`git init [project-name]`

#### Config

show current git config: `git config --list`

#### add/remove files

These files will be added or removed to `Index`.

`git rm/add [file1] [file2]`

`git mv [file-original] [file-renamed]`

#### Commit codes from `Index` to `Repository`

These commands will update all the changes from `Index` to `Repository`.

`git commit -m 'yourmessage'`

`git commit -v` to show all the differences.

#### Push codes from `Repository` to `Remote`

`git push [remote] [branch]` to push one of the local branch.

`git pull [remote] [branch]` to get the `Remote` codes into your workspace.

#### Show Information

`git status` to check all the changed files

`git log` to show the current branch's change log

`git diff` to show the differences between the `Workspace` and `Index`

`git diff HEAD` to show the difference between the `Workspace` and `Repository`

`git diff --shortstat "@{0 day ago}"` to show your daily coding lines number

## Matrix Sum Accelerator

This is the lab project in fall 2013 of UCB.

## Chisel3 Syntax

We can check the syntax at [its official website](https://www.chisel-lang.org/api/latest/chisel3/index.html). Here lists some high efficient and frequently used syntaxes. And also I printed [Chisel3 Cheat Sheet](https://www.chisel-lang.org/doc/chisel-cheatsheet3.pdf/) which greatly save my time.

+ `Vec`

+ `Seq` 

+ `Queen` 

+ `DecoupledIO`

+ `PriorityEncoder` 

+ `SyncReadMem` 

+ `Flipped` 

## (Linux) C Syntax

### `asm volatile`

`asm volatile` is the inline assembly that used in language Linux C. `asm` is the key word of `gcc` and `volatile` means `gcc` don't need to optimize the assemble codes. There is [a brief introduction](https://blog.csdn.net/slvher/article/details/8864996) of this. And I found this term while I was trying to [test the custom extensive instructions](#Instruction test with C) with `RoCC` interface. This syntax can run our own instruction without change the compiler. Plus, it can help our C program to run several commands in order with the help of [`asm volatile("" ::: "memory")`](https://blog.csdn.net/KISSMonX/article/details/9105823).

This syntax also has a feature named [Atomic](https://blog.csdn.net/wudiyi815/article/details/8468745). That means this command only has to states in runtime: either success or fail.

### create a C matrix with 2-D Array

[Define and initialize the matrix](https://blog.csdn.net/ZYTTAE/article/details/40017401):

```C
double matrix[10][15];
//initialize the static matrix
for (i=0;i<10;i++)
{
	for(j=0;j<15;j++)
	{
		matrix[i][j]=0;
	}
}


// define the struct, need pointer. m means row and n means column
typedef struct
{
	double **mat;
	int m, n;
}matrix;

//apply for memory space, use malloc()
void initial(matrix &T,int m,int n)
{
	int i = 0;
	T.mat = (double **)malloc(m*sizeof(double *));
	for (i = 0; i < m; m++)
	{
		T.mat[i] = (double *)malloc(n*sizeof(double));
	}
	T.m = m;
	T.n = n;
}

//initialize the matrix with the zero element.
void initValue(matrix &T, int m, int n)
{
	int i, j;
	initial(T,m,n);
	for (i = 0; i < m; i++)
	{
		for (j = 0; j < n; j++)
		{
			T.mat[i][j] = 0;
		}
	}
}

//free the memory space
void destroy(matrix &T)
{
	int i;
	for (i = 0; i < T.m; i++)
	{
		free(T.mat[i]);
	}
	free(T.mat);
}
//finished
```



### Pointer and address access

[This blog](https://blog.csdn.net/synapse7/article/details/10260339) describes and interprets the usage of `*` and `&` to get the address of one data or get the corresponding value of the address.

## RISC-V SPEC

## Chipyard

### `SHA3`

I mimicked `SHA3`, which also acts as one official sample to use `RoCC`. In order to write my own accelerator, I had to study the `SHA3` project.

If one wants to harness `RoCC` interface to add extensive instructions, he must design his Chisel hardware project firstly, and then write some C tests to prove this project works. Then modify some C files to let the compiler utilize your new instructions.

#### `SHA3`-Chisel

##### `sha3.scala`

This is the top module of `SHA3` project.

![RoCCinSHA3](https://images-cdn.shimo.im/ErYI4gMr0rkyBzVu/RoCCinSHA3.png)

##### `ctrl.scala`

This is the most complex submodule of `SHA3` project. It contains how to decode the instructions, as well as three finite-state machine. One (`rocc_s`) shows the states of the communication between the control module and the CPU(or receives instructions from CPU and uses ready-valid signals to show some states of the accelerator); One (`mem_s`) shows the states of the communication between the control module and the memory(or receives data from memory and send data back to memory and also ready-valid signals); The last one (`state`) shows the states of the communication between the control module and its submodules.

`dpath.scala`

This is the data path of the whole module.

`dmem.scala`

This is the module who responsible for transform data between memory and the accelerator. So we can use this module with just a slight modifying.

![dcache_req](https://images-cdn.shimo.im/Mz1ZvpxNzfoxeoSc/dcache_req.png)

![dcache_resp](https://images-cdn.shimo.im/cPCqqdGa1H84XK7w/dcache_resp.png)



#### `SHA3`-test



### `Hwacha`

`Hwacha` is another more complex sample of custom extensive instructions. Plus, it also used as a component to realize the `RISC-V` Standard Vector instructions. But it's quite complex and time tiny, so I didn't learn it thoroughly last week.

#### `Hwacha`-Chisel

## `RoCC` and extension instruction

### `RoCC` component

`RoCC` interface includes two main IO, one is for processor core and the other is for cache or memory. Due to the feature of Scala, we can use `io.cmd` and `io.mem` to show this. And both IOs own several kinds of signals, such as `io.cmd.valid` `io.cmd.ready` and `io.cmd.bits`, also the `io.mem.req` and `io.mem.resp`.

![RoCC interface](https://images-cdn.shimo.im/TG7ZkpEShygfsJDX/rocc_interface.png)

We can see it in more details from the photo bellow. **NOTE: this graph is a little old as it was published in 2015, and there are several slight changes.**

![RoCC 2015](https://images-cdn.shimo.im/ZhQo3Zs8BIQ8xrbL/RoCC2015.png)

### extension instruction with `RoCC`

The table bellow shows the `RoCC` instruction format.

![RoCC instruction format](https://images-cdn.shimo.im/2f63j2FAWmENrt14/rocc_instruction_format.png)

#### `RoCC`-Chisel

![RoCC IO](https://images-cdn.shimo.im/YhCOHPhZvhoGkkve/RoCCIO.png)

#### Instruction test with C

[ Tests for example Rocket Custom Coprocessors](https://github.com/seldridge/rocket-rocc-examples)

### `HellaCache`



## Chisel-Tester2

[main-page](https://github.com/ucb-bar/chisel-testers2)

## Hammer

[main-page](https://github.com/ucb-bar/hammer)

## Computer Architecture

### Memory Fence and Memory Barrier

### Page Table Walk

#### PTW Introduction



### Translation Lookaside Buffer

#### TLB Introduction

With the help of TLB, CPU may access the physical address [with just one accessment](https://blog.csdn.net/leishangwen/article/details/27190959). And it contains both some copies of page table and the physical address.

#### TLB passthrough

When I looked through the `TLB.scala` I found a valuable named `passrough`

### Cache Hierarchy

I found the following information when I tried to  understand the meaning of `memory tag`. I met this term  while I was trying to mimic [the control module](#SHA3-Chisel) of `SHA3`. Because it extends from [`HellaCache`](#HellaCache), which require tag. Although finally I found I can give out tag casually, I learned a lot from the structure of cache including TLB.

![Cache hierarchy of the K8 core in the AMD Athlon 64 CPU](https://pic3.zhimg.com/80/v2-0e77de9fe46e80c179204da1bf9ad6b2_hd.jpg)

The picture above illustrate the abstract architecture of cache hierarchy of one AMD CPU, from which we can see [`TLB`](#TLB) and Cache. Both these two components contains two levels, and save instructions and data separately.

If we see memory hierarchy excluding TLB, then it looks like the following picture:

![Cortex-A53 memory hierarchy](http://www-x-wowotech-x-net.img.abc188.com/content/uploadfile/201905/95101556803041.png)

And because of the dramatically booming speeds of high level caches, they can improve the performance of CPU. [We can regard](http://www.wowotech.net/memory_management/458.html) the cache is a method to improve the R/W speed of main memory, the CPU register is a method to improve that of the cache.

![cache speed](http://www-x-wowotech-x-net.img.abc188.com/content/uploadfile/201905/3d2f1556803040.jpg)

So how can the cache controller knows whether the data we need is included in current cache? So `tag` comes.

![the usage of tag](http://www-x-wowotech-x-net.img.abc188.com/content/uploadfile/201906/914f1559459886.png)

We can see from the graph above, there is a tag array and its corresponding data array. Firstly, controller will use index to find cache line, and then compares the tag array, then bingo.

[Sinaean Dean](https://zhuanlan.zhihu.com/p/31875174) introduced the two usages of cache tag:

+ For caches parallelly connected together, using tags as a selection signal in a MUX to choose the right data.

+ Know whether cache hits or misses.

If the cache miss, then it will ask a lower lever cache for the data or instructions.

![cooperation between different levels of caches](http://www-x-wowotech-x-net.img.abc188.com/content/uploadfile/201905/560d1556803043.png)

### Memory Read (Synchronous and Asynchronous)
[This blog](https://www.cnblogs.com/kalo1111/p/3245056.html) describes the [synchronous memory](#Chisel3 Syntax) -- SSRAM, which is controlled by clock and can not read or write at the same clock cycle. But if we use asynchronous memory, then we can get the read data within one clock and if we use a combinational/asynchronous-read, sequential/synchronous-write memory, then the read-after-write hazards [are not an issue](https://www.chisel-lang.org/api/latest/chisel3/Mem.html).

## Some Trouble

### `psutil`

ERROR INFO: `No module named psutil`.

But I have installed this module. Then via [some blogs](https://blog.csdn.net/weixin_42348333/article/details/85068701), I find that might be the version of python that excursive doesn't install the module. So I [changed the default version of python](https://blog.csdn.net/u011534057/article/details/51615193) from 2.7 to 3.5 and reinstall it with `pip3`.

### Import classes into `IntelliJ IDEA`

I use some modules of `chipyard`, which means I have to import that project into my project.

At the beginning, I just add the files which contain the Scala files directly, but it didn't work at all. Because the packages and classes are defined at a more high level.

By accidently, I included highest file (`generator` in this case) and it succeed.

### Import reliance into `sbt` project

```scala
lazy val matrixsum = conditionalDependsOn(project in file("/home/singularity/chipyard/generators/matrixsum"))
	.dependsOn(rocketchip, chisel_testers, sifive_blocks, sifive_cache, utilities, midasTargetUtils)
	.settings(commonSettings)
```

But that's works on the top module, the `dependson` must be a sub file of the current `build.sbt`  directory.

### Can not find the lvy local lib

Haven't solved yet.

```
1 targets failed
adder.compileClasspath 
Resolution failed for 1 modules:
--------------------------------------------
	edu.berkeley.cs:treadle_2.12:1.2-SNAPSHOT 
	not found: /home/singularity/.ivy2/local/edu.berkeley.cs/treadle_2.12/1.2-SNAPSHOT/ivys/ivy.xml
	not found: https://repo1.maven.org/maven2/edu/berkeley/cs/treadle_2.12/1.2-SNAPSHOT/treadle_2.12-1.2-SNAPSHOT.pom
	not found: https://oss.sonatype.org/content/repositories/releases/edu/berkeley/cs/treadle_2.12/1.2-SNAPSHOT/treadle_2.12-1.2-SNAPSHOT.pom
```

