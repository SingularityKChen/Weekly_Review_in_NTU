# Table of Contents

<!-- MarkdownTOC -->

1. [2020/01/06-12](#20200106-12)
	1. [EIE: Efficient Inference Engine on Compressed Deep Neural Network](#eie-efficient-inference-engine-on-compressed-deep-neural-network)
		1. [Exploit the Sparsity of Activations with Compressed Sparse Column \(CSC\) Format](#exploit-the-sparsity-of-activations-with-compressed-sparse-column-csc-format)
		1. [Parallelizing Compressed DNN](#parallelizing-compressed-dnn)
		1. [Hardware Implementation](#hardware-implementation)
		1. [Design Space Exploration](#design-space-exploration)
	1. [ProxylessNAS: Direct Neural Architecture Search on Target Task and Hardware](#proxylessnas-direct-neural-architecture-search-on-target-task-and-hardware)
		1. [Method](#method)
		1. [Make Latency Differentiable](#make-latency-differentiable)
	1. [Design Automation for Efficient Deep Learning Computing](#design-automation-for-efficient-deep-learning-computing)
		1. [Automated Model Specialization](#automated-model-specialization)
		1. [Automated Channel Pruning](#automated-channel-pruning)
		1. [Automated Mixed-Precision Quantization](#automated-mixed-precision-quantization)
	1. [Learning to Design Circuits](#learning-to-design-circuits)
	1. [Chisel & Scala Syntax](#chisel--scala-syntax)
		1. [`Chisel.Queue`](#chiselqueue)
		1. [`Chisel.Decoupled`](#chiseldecoupled)
		1. [`Chisel.suggestName`](#chiselsuggestname)
		1. [`Chisel.print`](#chiselprint)
		1. [`Scala.zip`](#scalazip)
		1. [`Scala.map`](#scalamap)
		1. [`Scala.reduce` \(`reduceLeft` & `reduceRight`\)](#scalareduce-reduceleft--reduceright)
		1. [`Scala.List.take(Int)`](#scalalisttakeint)
		1. [Reg Delay Test](#reg-delay-test)
1. [2019/12/30-2020/01/05](#20191230-20200105)
	1. [A Survey on Methods and Theories of Quantized Neural Networks](#a-survey-on-methods-and-theories-of-quantized-neural-networks)
		1. [Quantization Techniques](#quantization-techniques)
		1. [Two Quantization Methodologies](#two-quantization-methodologies)
		1. [Discussion](#discussion)
		1. [Why Does Quantization Work?](#why-does-quantization-work)
		1. [Future of Quantized Neural Networks](#future-of-quantized-neural-networks)
	1. [Some Terms](#some-terms)
		1. [Stackelberg Game](#stackelberg-game)
		1. [Fog Computation](#fog-computation)
1. [2019/12/23-29](#20191223-29)
	1. [Efficient Processing of Deep Neural Networks: A Tutorial and Survey](#efficient-processing-of-deep-neural-networks-a-tutorial-and-survey)
		1. [Creating A System for Efficient DNN Processing](#creating-a-system-for-efficient-dnn-processing)
		1. [Temporal Architectures and Spatial Architectures](#temporal-architectures-and-spatial-architectures)
		1. [Near-Data Processing](#near-data-processing)
		1. [Co-Design of DNN Models And Hardware](#co-design-of-dnn-models-and-hardware)
		1. [Benchmarking Metrics for DNN Evaluation and Comparison](#benchmarking-metrics-for-dnn-evaluation-and-comparison)
	1. [Eyeriss v2: A Flexible and High-Performance Accelerator for Emerging Deep Neural Networks](#eyeriss-v2-a-flexible-and-high-performance-accelerator-for-emerging-deep-neural-networks)
		1. [Changes in Performance and Flexibility](#changes-in-performance-and-flexibility)
		1. [Eyexam](#eyexam)
		1. [Flexible Dataflow](#flexible-dataflow)
		1. [Flexible and Scalable NoC](#flexible-and-scalable-noc)
	1. [Eyeriss: A Spatial Architecture for Energy-Efficient Dataflow for Convolutional Neural Networks](#eyeriss-a-spatial-architecture-for-energy-efficient-dataflow-for-convolutional-neural-networks)
		1. [Framework for Energy Efficiency Analysis](#framework-for-energy-efficiency-analysis)
	1. [Eyeriss: An Energy-Efficient Reconfigurable Accelerator for Deep Convolutional Neural Networks](#eyeriss-an-energy-efficient-reconfigurable-accelerator-for-deep-convolutional-neural-networks)
		1. [Main Features of Eyeriss](#main-features-of-eyeriss)
		1. [System Control and Configuration](#system-control-and-configuration)
		1. [Exploit Data Statistics](#exploit-data-statistics)
	1. [Some Troubles](#some-troubles)
		1. [Scalac_2.12.4:4.2.3](#scalac_2124423)
		1. [No found: firrlt object](#no-found-firrlt-object)
1. [2019/12/16-22](#20191216-22)
	1. [Functional Programming Principles in Scala](#functional-programming-principles-in-scala)
		1. [Week1](#week1)
		1. [Week2](#week2)
	1. [Efficient Processing of Deep Neural Network: from Algorithms to Hardware Architectures](#efficient-processing-of-deep-neural-network-from-algorithms-to-hardware-architectures)
1. [2019/12/09-15](#20191209-15)
	1. [Git Command](#git-command)
		1. [the workflow](#the-workflow)
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

---



# 2020/01/06-12

This week I read several papers written by Song Han including [reference](https://github.com/SingularityKChen/Weekly_Review_in_NTU/blob/master/Reference.md) 12, 15-17, related to both deep learning accelerator architecture with Weight Stationary and design automation for Neural Architecture Search.

Also, I began the implementation of [Processing Element](https://github.com/SingularityKChen/dl_accelerator), and finished the main function without any test.

So next week, I plan to finish the PE with a test file, and then establish the analysis system based on [Eyexam](#eyexam) with some scripts to compute the details of hardware. So I need to re-read the papers related to [Eyeriss](#20191223-29).

---

## EIE: Efficient Inference Engine on Compressed Deep Neural Network

![Efficient inference engine that works on the compressed deep neural network model](https://images-cdn.shimo.im/7hWMl33GZigEvWNW/image.png)

### Exploit the Sparsity of Activations with Compressed Sparse Column (CSC) Format
For each column $W_j$ of matrix $W$ we store a vector $v$ that contains the non-zero weights, and a second, equal-length vector $z$ that encodes the number of zeros before the corresponding entry in $v$. Each entry of  $v$ and $z$ is represented by a four-bit value. If more than 15 zeros appear
before a non-zero entry we add a zero in vector $v$.

![Memory layout for the relative indexed](https://images-cdn.shimo.im/2LMlD7Bu84UsueOG/image.png)

### Parallelizing Compressed DNN

perform the sparse matrix  sparse vector operation by scanning vector $a$ to find its next non-zero value  $a_j$and broadcasting $a_j$ along with its index $j$ to all PEs. Each PE then multiplies $a_j$ by the non-zero elements in its portion of column $W_j$ — accumulating the partial sums in accumulators for each element of the output activation vector b. In the CSC representation these non-zeros weights are stored contiguously so each PE simply walks through its $v$ array from location $p_j$ to $p_{j+1}-1$ to load the weights. To address the output accumulators, the row number $i$ corresponding to each weight $W_{ij}$ is generated by keeping a running sum of the entries of the x array.

![](https://images-cdn.shimo.im/8Y6M5PoCqV01NZdy/image.png)

The interleaved CSC representation allows each PE to quickly find the non-zeros in each column to be multiplied by $a_j$ . This organization also keeps all of the computation except for the broadcast of the input activations local to a PE.

### Hardware Implementation

![](https://images-cdn.shimo.im/Nr3mEv6LW0Y31KxB/image.png)

+ Activation Queue and Load Balancing: The broadcast is disabled if any PE has a full queue. At any point in time each PE processes the activation at the head of its queue.
+ Pointer Read Unit: $p_j$ and $p_{j+1}$ will always be in different banks.
+ Sparse Matrix Read Unit: uses pointers $p_j$ and $p_{j+1}$ to read the non-zero elements
+ Arithmetic Unit: performs the multiply-accumulate operation
+ Activation Read/Write: contains two activation register files that accommodate the source and destination activation values respectively during a single round of FC layer computation
+ Distributed Leading Non-Zero Detection: select the first non-zero result. The result is sent to a Leading Nonzero Detection Node (LNZD Node)
+ Central Control Unit: It communicates with the master and monitors the state of every PE by setting the control registers. There are two modes in the Central Unit: I/O and Computing.
  +  In the I/O mode, all of the PEs are idle while the activations and weights in every PE can be accessed by a DMA connected with the Central Unit. This is one time cost. 
  + In the Computing mode, the CCU repeatedly collects a non-zero value from the LNZD quadtree and broadcasts this value to all PEs.

brought the critical path delay down to 1.15ns by introducing 4 pipeline stages to update one activation:

+ codebook lookup and address accumulation (in parallel)
+ output activation read and input activation multiply (in parallel)
+ shift and add
+ output activation write.

### Design Space Exploration

**Queue Depth**. The activation FIFO queue deals with load imbalance between the PEs. A deeper FIFO queue can better decouple producer and consumer.

**SRAM Width**. We choose an SRAM with a 64-bit interface to store the sparse matrix (`Spmat`) since it minimized the total energy. Wider SRAM interfaces reduce the number of total SRAM accesses, but increase the energy cost per SRAM read.

**Arithmetic Precision**. We use 16-bit fixed-point arithmetic. 16-bit fixed-point multiplication consumes 5 times less energy than 32-bit fixed-point and 6.2 times less energy than 32-bit floating-point.

## ProxylessNAS: Direct Neural Architecture Search on Target Task and Hardware

ProxylessNAS is the first NAS algorithm that directly learns architectures on the largescale dataset (e.g. ImageNet) without any proxy while still allowing a large candidate set and removing the restriction of repeating blocks. it is the first work to study specialized neural network architectures for different hardware architectures.

![optimizes neural network architectures](https://images-cdn.shimo.im/qiQowKgCctgUyfLd/image.png)

### Method

+ describe the construction of the over-parameterized network with all candidate paths
+ introduce how we leverage binarized architecture parameters to reduce the memory consumption of training the over-parameterized network to the same level as regular training.

![Learning both weight parameters and binarized architecture parameters](https://images-cdn.shimo.im/GtQyHdVEM7UiZmh7/image.png)

+ first freeze the architecture parameters and stochastically sample binary gates according to Eq. (2) for each batch of input data
+ the weight parameters of active paths are updated via standard gradient descent on the training set (Figure 2 left)
+ When training architecture parameters, the weight parameters are frozen
+ reset the binary gates and update the architecture parameters on the validation set (Figure 2 right). These two update steps are performed in an alternative manner. 
+ derive the compact architecture by pruning redundant paths

### Make Latency Differentiable

Model the latency of a network as a continuous function of the neural network dimensions.

![Making latency differentiable by introducing latency regularization loss](https://images-cdn.shimo.im/4wwTe3kgMO4AJleJ/image.png)

## Design Automation for Efficient Deep Learning Computing

![Design automation for model specialization, channel pruning and mixed-precision quantization](https://images-cdn.shimo.im/anETjNuh440NbKou/image.png)

Three main points in this paper:

+ automatically designing specialized fast models
+ auto channel pruning
+ auto mixed-precision quantization

### Automated Model Specialization

In order to fully utilize the hardware resource, start with a large design space (Figure 1(a)) that includes many candidate paths to learn which is the best one by gradient descent, rather than hand-picking with rule-based heuristics.

The search space for each block $i$ consists of many choices:

+ `ConvOp`: mobile inverted bottleneck conv [9] with various kernel sizes and expansion ratios
  + Kernel size: {3\*3, 5\*5, 7\*7}
  + Expansion ratio: {3, 6}
+ `ZeroOp`: if `ZeroOp` is chosen at $i^{th}$ block, it means the block is skipped.

In the forward step, to save GPU memory, we allow only one candidate path to actively reside in the GPU memory. This is achieved by hard-thresholding the probability of each candidate path to either 0 or 1 (i.e., path-level binarization).

### Automated Channel Pruning

Pruning too much will hurt accuracy; too less will not achieve high compression ratio.

Our automatic model compression (AMC) leverages reinforcement learning to efficiently search the pruning ratio.

We train an reinforcement learning agent to predict the best sparsity for a give hardware. We evaluate the accuracy and FLOPs after pruning. Then we update the agent by encouraging smaller, faster and more accurate models.

The easiest way to reduce the channels of a model is to use uniform channel shrinkage, i.e. use a width multiplier to uniformly reduce the channels of each layer with a fixed ratio.

### Automated Mixed-Precision Quantization

Our hardware-aware automatic quantization (HAQ) models the quantization task as a reinforcement learning problem. We use the actor-critic model to give the quantization policy (#bits per layer) (Figure 1(c)). The goal is not only high accuracy but also low energy and low latency.

Inferencing on edge devices and cloud severs can be quite different, since:

+ the batch size on the cloud servers are larger
+ the edge devices are usually limited to low computation resources and memory bandwidth.

Interpret the quantization policy’s difference between edge and cloud by the roofline model.

## Learning to Design Circuits

Two difficult for searching for parameters that satisfy circuit specifications due to the low availability of training data:

+ Circuit simulation is slow, thus generating large-scale dataset is time-consuming
+ Most circuit designs are propitiatory IPs within individual IC companies, making it expensive to collect large-scale datasets

Constrains for L2DC(Learning to Design Circuits):

+ meet hard-constraints (eg. gain, bandwidth)

+ optimize good-to-have targets (eg. area, power)

![Learning to Design Circuits (L2DC) Method Overview](https://images-cdn.shimo.im/DygvZ9uF87sLxhUy/image.png)

Steps:

+ leverages reinforcement learning (RL) to generate circuits data by itself and learns from the data to search for best parameters.

+ produces an action (a set of parameters) to the circuit simulator environment, and then receives a reward as a function of gain, bandwidth, power, area, etc.
+ The reward is defined to optimize the desired Figures of Merits (FOM) composed of several performance metrics.
+ By maximizing the reward, RL agent can optimize the circuit parameters.

![use sequence to sequence model to generate circuit parameters](https://images-cdn.shimo.im/AY5WaAFy94ct6a5z/image.png)

## Chisel & Scala Syntax

Useful resource:

Chisel [Wiki](https://github.com/freechipsproject/chisel3/wiki/CS250-Chisel+Scala-Primer)

Chisel [CookBook](https://github.com/freechipsproject/chisel3/wiki/Cookbook)

Chisel3 [API](https://www.chisel-lang.org/api/latest/chisel3/index.html)

[Runoob Scala tutorial](https://www.runoob.com/scala/scala-tutorial.html)

Scala language [PDF](https://static.runoob.com/download/Scala%E8%AF%AD%E8%A8%80%E8%A7%84%E8%8C%83.pdf)

### `Chisel.Queue`

```scala
val qa = Queue(io.a) // io.a is the input to the FIFO
                     // qa is DecoupledIO output from FIFO

```



### `Chisel.Decoupled`

```scala
val b = Flipped(Decoupled(UInt(32.W)))// valid and bits are inputs
val z = Decoupled(UInt(32.W)) // valid and bits are outputs
```



### `Chisel.suggestName`

```scala
  def inc(x: UInt): UInt = {
    val incremented = x + 1.U // We cannot name this, it's inside a method
    incremented.suggestName("incremented") // Now it is named!
  }
```



### `Chisel.print`

```scala
println(s"${io.in}") 
// chisel3.core.UInt@e

printf(p"$io") 
// Bundle(in->3, out->3)

println(s"out = ${peek(c.io.out)}") 
// out = Vector(0, 0, 0, 0, 0, 0)
```



### `Scala.zip`

Pair up values in two lists.

```scala
scala> List("a", "b", "c").zipWithIndex
res0: List[(String, Int)] = List((a,0), (b,1), (c,2))

scala> List("a", "b", "c") zip (Stream from 1)
res1: List[(String, Int)] = List((a,1), (b,2), (c,3))

scala> List(1, 2, 3).zip(List("a", "b", "c"))
res2: List[(Int, String)] = List((1,a), (2,b), (3,c))
```



### `Scala.map`

Applies a function to each element in a list and returns the resulting transform in a list

```scala
scala> val list1 = 0 to 5 toList
res3: List[Int] = List(0, 1, 2, 3, 4, 5)
 
scala> list1.map(x=>x*x)
res4: List[Int] = List(0, 1, 4, 9, 16, 25)
```



### `Scala.reduce` (`reduceLeft` & `reduceRight`)

In each step, a function is preformed on the list element and the result of the last fold operation, returning a single value.

```scala
scala> val list1 = 0 to 5 toList
res51: List[Int] = List(1, 2, 3, 4, 5)

scala> val sum = (x:Int, y:Int) => {println(x,y) ; x + y}
sum: (Int, Int) => Int = <function2>

scala> list1.reduce(sum)
(1,2) // 1 + 2 =  3, list1 = List(3, 3, 4, 5)
(3,3) // 3 + 3 =  6, list1 = List(6, 4, 5)
(6,4) // 6 + 4 = 10, list1 = List(10, 5)
(10,5)
res52: Int = 15
```



### `Scala.List.take(Int)`

See [Scala List](https://www.runoob.com/scala/scala-lists.html).

```scala
val mcrenf = RegInit(VecInit(Seq.fill(6)(0.U(loop_width.W))))
val when_carry: IndexedSeq[Bool] = mcrenf.zip(MCRENF.map(x=> x - 1)).map{ case (x,y) => x === y.U}

when_carry.take(i).reduce(_ && _)
```



### Reg Delay Test

```scala
class TheOperModule extends Module {
  val io = IO(new Bundle {
    val a  = Input(UInt(4.W))
    val b  = Input(UInt(4.W))
    val out_reg0 = Output(UInt(4.W))
    val out_reg1 = Output(UInt(4.W))
    val out_reg2 = Output(UInt(4.W))
  })

    val a_reg = RegInit(0.U(4.W))
    val b_reg = RegInit(0.U(4.W))
    val c_reg1 = RegInit(0.U(4.W))
    val d_reg2 = RegInit(0.U(4.W))
    a_reg := io.a
    b_reg := io.b
    c_reg1 := io.a + io.b
    d_reg2 := a_reg + b_reg
  
    io.out_reg0 := io.a + io.b
    io.out_reg1 := c_reg1
    io.out_reg2 := d_reg2
}
class MyOperTester(c: TheOperModule) extends PeekPokeTester(c) {
    val in_a = 2
    val in_b = 3
    poke(c.io.a, in_a)
    poke(c.io.b, in_b)
    println(s"clock 0, out_reg0 = ${peek(c.io.out_reg0)}")
    println(s"         out_reg1 = ${peek(c.io.out_reg1)}")
    println(s"         out_reg2 = ${peek(c.io.out_reg2)}")
    step(1)
    println(s"clock 1, out_reg0 = ${peek(c.io.out_reg0)}")
    println(s"         out_reg1 = ${peek(c.io.out_reg1)}")
    println(s"         out_reg2 = ${peek(c.io.out_reg2)}")
    step(1)
    println(s"clock 2, out_reg0 = ${peek(c.io.out_reg0)}")
    println(s"         out_reg1 = ${peek(c.io.out_reg1)}")
    println(s"         out_reg2 = ${peek(c.io.out_reg2)}")
}
assert(Driver(() => new TheOperModule) {c => new MyOperTester(c)})
```

```
[info] [0.001] clock 0, out_reg0 = 5
[info] [0.001]          out_reg1 = 0
[info] [0.001]          out_reg2 = 0
[info] [0.001] clock 1, out_reg0 = 5
[info] [0.001]          out_reg1 = 5 // arrive reg 1
[info] [0.002]          out_reg2 = 0
[info] [0.002] clock 2, out_reg0 = 5
[info] [0.002]          out_reg1 = 5
[info] [0.002]          out_reg2 = 5 // arrive reg 2
```



# 2019/12/30-2020/01/05

Last week, I only read one article which is the survey of QNN. Although I know how to utilize several QNN models to reduce the memory requirements and power consumption, I don't understand why those formulas works  due to the lack of some basic mathematic knowledge in neural network.

I took my main time and energy to write my initial stage's report of my final year's project. During that period, I recall the paper I read the past month and thought over my project in a more detail. I though I could start my project with the basic processing element and then read more paper to find something interesting and then optimize the design.

Also, before the new year, I created one new Linux virtual machine with Virtual Box. And after new year, I downloaded the latest Chipyard project from GitHub and rebuilt the environment in both mine virtual machine and Shien's. Fortunately, the new environment works now.

And this week, I will try to read as many Song Han's papers as possible before his seminar, and then I will read some other papers [listed before](#20191216-22). I hope I could finish the translation work of chisel-bootcamp module 3.6 before this Saturday. After that, I will begin the design of PE in my FYP.

---

## A Survey on Methods and Theories of Quantized Neural Networks

### Quantization Techniques

#### Deterministic quantization

![Summarization of different quantization methods](https://images-cdn.shimo.im/E83E8CAU8qoHf9i7/image.png)

+ Rounding 

![rounding function](https://images-cdn.shimo.im/58vtJu1ZLs8MoiXT/image.png)

+ Vector Quantization: cluster the weights into groups and use the centroid of each group to replace the actual weights during inference.

	 ![](https://images-cdn.shimo.im/tYIO1b2acTYLsA2f/image.png)

	+ k-means
	+ Hessian weighted k-means
	+ product quantization
	+ residual quantization

+ Quantization as Optimization: formulating the quantization problem as an optimization problem

#### Stochastic Quantization

+ Random Rounding: In random rounding, the real value has no one-to-one mapping to the quantized value. Typically, the quantized value is sampled from a discrete distribution which is parameterized by the real values.

![random rounding function](https://images-cdn.shimo.im/Nj7hgrxuuyIB8VSC/image.png)

+ Probabilistic Quantization: the weights are assumed to be discretely distributed.

### Two Quantization Methodologies
![Summarization of fixed codebook and adaptive codebook quantization](https://images-cdn.shimo.im/Lcm0g5YaotI8sPTx/image.png)

#### Fixed codebook quantization

The codebook is predefined.

#### Adaptive Codebook Quantization

The codebook is learned from the data. Vector quantization and probabilistic quantization are two possible methods to achieve adaptive codebook quantization.

Two kinds of adaptive codebook quantization exist: hard quantization and soft quantization. 

+ In hard quantization, the real value is assigned exactly to be one of the discrete values. 
+ In soft quantization the real value is assigned to be some discrete value according to a probability distribution.

### Discussion

Quantized weights and activations occupy less memory compared with full-precision counterparts. Meanwhile, the training and inference speed can be greatly accelerated since the dot products
between weights and activations can be replaced by bitwise operations. Quantized gradients can reduce the overhead of gradient synchronization in parallel neural network training. In a single worker scenario, quantized gradients can accelerate back-propagation training as well as requiring less memory.

### Why Does Quantization Work?

They found that the binarization operation preserves some important properties of the full-precision networks, i.e,

+ Angle Preservation Property
+ Dot Product Proportionality Property

### Future of Quantized Neural Networks

The following possible directions for the next steps:

+ Develop more sophisticated rounding mechanism to train quantized neural network from scratch. One possible approach is to use the structure information of the weights to guide the rounding process.
+ Design quantized neural networks for tasks such as natural language processing, speech recognition and so on. Due to the varieties of deep learning models, a generally applicable quantization method is necessary.
+ Develop theoretical guidance for quantizing neural networks.

## Some Terms

### Stackelberg Game

In the [MBAlib](https://wiki.mbalib.com/wiki/%E6%96%AF%E5%A1%94%E5%85%8B%E5%B0%94%E4%BC%AF%E6%A0%BC%E6%A8%A1%E5%9E%8B) describes the meaning of it.

The **Stackelberg leadership model** is a strategic game in economics in which the leader firm moves first and then the follower firms move sequentially.

### Fog Computation



# 2019/12/23-29

Last week, I read four Journal Articles related to Deep Learning Accelerator Implementation. Thanks to Shien's recommendation, I learned a lot from these articles as I have a more vivid knowledge of DLA with several in-deep views in some components. 

When I reviewed these four articles, I found that even though I know how to design the hardware architecture, I may not know how to run some DNN models with the accelerator. Letty told me that they need the help of some compilers to translate the models to instructions, but I don't think I could optimize the compiler to compile the program into my custom instructions. So maybe I would not run a complex model if I can not load instructions manually.

And next week, I am supposed to read some papers which describe the method to compress the different types of data in CNN, i.e., QNN, to decrease the movements of data and the storage requirements of its four levels of the memory hierarchy. Besides, Chen Peng has introduced me thinking that routers can also be regards as one of the calculation components in NoC, so maybe the next few weeks, I will improve the dataflow of Eyeriss with this kind of pattern.

---

## Efficient Processing of Deep Neural Networks: A Tutorial and Survey

This article focuses on:

+ processing of DNN inference
+ addressing the efficiency of the CONV layers

### Creating A System for Efficient DNN Processing

+ Applications and specific computations requirements
+ Understand and balance the important system metrics
	+ Accuracy
	+ Energy
	+ Throughput
	+ Hardware cost
+ Optimize DNN processing
+ Joint hardware/software co-design

### Temporal Architectures and Spatial Architectures

![Temporal Architectures and Spatial Architectures](https://images-cdn.shimo.im/Y0FJBfLpHro9PDzw/image.png)

#### Temporal Architectures

+ appear mostly in CPUs or GPUs
+ employ a variety of techniques to improve parallelisms such as vectors (SIMD) or parallel threads (SIMT)
+ use a centralized control for a large number of ALUs
+ can only fetch data from the memory hierarchy
+ cannot communicate directly with each other
+ reduce the number of multiplications to increase throughput

#### Spatial Architectures

+ use dataflow processing can pass data from one to another directly
+ ALU can have its own control logic and local memory
+ how dataflows can increase data reuse from low-cost memories in the memory hierarchy to reduce energy consumption

#### Apply Computational Transforms to The Data

+ Fast Fourier Transform (FFT) from O( $N^2_o$ $N^2_f$ ) to O($N^2_o$ $log_2N_o$) 
+ Winograd’s algorithm
+ Strassen’s algorithm from O($N^3$) to O($N^{2.807}$)

#### Energy-Efficient Dataflow for Accelerators

Each MAC requires three memory reads:

+ filter weight
+ fmap activation
+ partial sum

And one memory write: updated partial sum.

Different Dataflow:

+ Weight Stationary *(WS)*
+ Output Stationary *(OS)*
+ No Local Reuse *(NLR)*
+ Row Stationary *(RS)*

### Near-Data Processing

We are supported to reduce data movement by moving compute and data closer. For example, 

+ Integrate the computation into the memory itself
+ Bring the computation into the sensor where the data are first collected

#### DRAM

Avoid off-chip access by using high-density memories such as DRAMs. which can store tens of megabytes of weights and activations on-chip.

Also, with the help of through-silicon vias *(TSVs)*, or 3-D memory, and HMC, HBM, the DRAM can also be stacked on the top of the chip.

#### SRAM

Bring the compute into the memory.

#### Non-volatile Resistive Memories

+ resistor’s conductance as the weight
+ the voltage as the input
+ the current as the output
+ the addition is done by Kirchhoff’s current law

But it has some cons:

+ suffers from the reduced precision as it needs ADC/DAC
+ the array size is limited by the wires
+ the IR drop along wire can degrade the read accuracy

#### Sensors

Need to move the computation into the analogy domain to avoid using the ADC within the sensor.

### Co-Design of DNN Models And Hardware

#### Reduce Precision

And we can apply different precisions into weights and activations, different layers.

Reduce the precision of operations and operands:

+ fixed point instead of floating-point
+ reducing the bit-width: uniform quantization
+ nonuniform quantization: map data to a smaller set of quantization levels
	+ log quantization
	+ learned quantization
+ weight sharing

#### Reduced Number of Operations and Model Size

Reduce the number of operations and model size:

+ compression: exploiting activation statistics
+ network pruning
+ compact network architectures: replace a large filter with a series of small filters

### Benchmarking Metrics for DNN Evaluation and Comparison

#### Metrics for DNN Models

+ The accuracy
+ The network architecture of the model including the number of layers, filter sizes, number of filters and number of channels
+ The number of weights: impact the storage requirement
+ The number of MACs: potential throughput

#### Metrics for DNN Hardware

+ The power  and energy consumption
+ The latency and throughput
+ The cost of the chip: the size and type of memory, the amount of control logic
+ For an FPGA, 
	+ the specific device
	+ the utilization of resources such as:
		+ DSP
		+ BRAM
		+ LUT
		+ FF
	+ performance density
		+ GOPs/slice

## Eyeriss v2: A Flexible and High-Performance Accelerator for Emerging Deep Neural Networks

This article describes:

+ a performance analysis framework named *`Eyexam`* which provides a seven-step process to systematically identify the source of performance loss and can be used to develop a set of `roofline models` to quantitatively identify the source of performance loss in DNN processors;
+ a DNN accelerator named *`Eyeriss v2`* which uses a hierarchical `NoC` that allows the system to exploit spatial data reuse for large DNNs and deliver high bandwidth for compact DNNs with the scalability;

### Changes in Performance and Flexibility

#### Two Bad Ways in Widely Varying Data Reuse

+ depend on data reuse in certain data dimensions
	+ the spatial accumulation array architecture relies on both output and input channels;
	+ the temporal accumulation array architecture relies on another set of data dimensions;
+ lower data reuse need a higher data bandwidth

#### To Build a Truly Flexible DNN Processor

+ the dataflow relies on certain data dimensions for data reuse
	+ low utilization of the parallelism when those data dimensions diminish
+ the NoC and its corresponding memory hierarchy
	+ high bandwidth and low spatial data reuse;
	+ low bandwidth and high spatial data reuse scenarios;
	+ instead of being adaptive to the specific condition of the workload;

### Eyexam

#### Two Main Factors that Affect Performance

+ the number of active PEs due to the mapping determined by the dataflow;
+ the utilization of active PEs based on whether the NoC has sufficient bandwidth to deliver data to PEs to keep them active;

This is a example dataflow for a 1D CONV:

```c
#include <stdio.h>

int main()
{
	int R2, R1, R0, E2, E1, E0;
	// R0, R1, R2 related to filter
	R2 = 2;//chnl of filter, C, sum of one chnl ofmap
	R1 = 3;//height of filter, R, psum of one row ofmap
	R0 = 4;//width of filter, S, ppsum of one ofmap
	// E0, E1, E2 related to ofmap
	E2 = 2;//chnl of ofmap or number of filters, M, sums in different chnl ofmaps
	E1 = 4;//height of ofmap, E, psum of one chnl ofmap
	E0 = 5;//width of ofmap, F, ppsum of one row ofmap
	int inx_o, inx_i, inx_w;
	for (int e2 = 0; e2 < E2; e2++) {
		for (int r2 = 0; r2 < R2; r2++) {
			for (int e1 = 0; e1 < E1; e1++) {
				for (int r1 = 0; r1 < R1; r1++) {
					for (int e0 = 0; e0 < E0; e0++) {
						for (int r0 = 0; r0 < R0; r0++) {
							inx_o = e2*E1*E0+e1*E0+e0;
							inx_i = e2*E1*E0+e1*E0+e0 + r2*R1*R0+r1*R0+r0;
							inx_w = r2*R1*R0+r1*R0+r0;
							printf("O[%d] += I[%d] * W[%d]\t", inx_o, inx_i, inx_w);
							//printf("O[e2*E1*E0+e1*E0+e0] = O[%d*E1*E0+%d*E0+%d]\n", e2, e1, e0);
							/* code */
						}
						/* code */
						//printf("----------- r0 for loop finished -----------\n");
						printf("\n");
					}
					/* code */
					printf("*********** e0 for loop finished ***********\n");
				}
				/* code */
				printf("+++++++++++ r1 for loop finished +++++++++++\n");
			}
			/* code */
			printf("=========== e1 for loop finished ===========\n");
		}
		/* code */
		printf("@@@@@@@@@@@ r2 for loop finished @@@@@@@@@@@\n");
	}
	return 0;
}

```

+ inner two `for` loops represent the temporal processing and `SPad` accesses within a `PE`;
+ the outer two `for` loops represent the temporal processing of multiple passes across the `PE` array and GLB accesses;
+ the two `parallel-for`s represent the distribution of computation across multiple `PE`s;

#### Seven steps to analyse the performance of an architecture

+ layer shape and size: the number of `MAC`s in the layer
+ dataflow: the maximum parallelism of the dataflow
+ number of `PE`s: the theoretical peak performance. Two shape fragmentation:
	+ spatial mapping fragmentation: `R` or `R1` is smaller than the number of `PE`s => some are completely idle
	+ temporal mapping fragmentation: `R` is not an integer multiple of `PE`s => some are not always active
+ physical dimensions of the `PE` array: the run-time utilization of `PE`s due to shape fragmentation for each dimension
+ storage capacity: reduce the number of active `PE`s due to storage of intermediate data. e.g., limited storage for `psum`s => the limited number of weights be processed in parallel => limited number of active `PE`s that can operate in parallel
+ data bandwidth: insufficient average bandwidth to active `PE`s. The performance will be bandwidth-limited when the operational intensity is lower than the inflection point
+ varying data access patterns: insufficient instant bandwidth to active `PE`s

![the roofline model](https://images-cdn.shimo.im/C9YNzwIHSE87b2sJ/image.png)

![the impact of Eyexam steps on the roofline model](https://images-cdn.shimo.im/OJ7PwTSeEwIakuk9/image.png)

![summary of steps in Eyexam](https://images-cdn.shimo.im/baEhAdv31Z0zD03y/image.png)

#### Performance Analysis Results for DNN Processors and Workloads
+ simply increasing hardware resources is not sufficient to achieve a higher performance
+ the dataflow has to be flexible enough to deal the diminished reuse available in any data dimensions
+ the NoC design should meet the worst-case bandwidth requirement for every data type
+ the NoC design should aim to exploit data reuse to minimize the number of GLB accesses

### Flexible Dataflow

The new dataflow named *`Row-Stationary Plus`* supports data tiling from all dimensions to fully utilize the PE array.

+ additional loops *`g1`* and *`n1`* are added at the NoC level to provide more options to parallelize different dimensions of data
+ allow data to be tiled from multiple dimensions and mapped in parallel onto the same `PE` array dimension
+ the data tile from the same dimension can also be mapped spatially onto different physical dimensions to fully utilize the `PE` array
+ allow tile in data dimension *`R`* with loop *`r1`*
+ an additional loop *`e0`* is added at the `SPad` level to create more reuse of weights in the `SPad`.

![The definition of the Row-Stationary Plus (RS+) dataflow](https://images-cdn.shimo.im/Lr26hZAgvUcKEqu7/image.png)

```c
inx_o = g3*G2*G1 + g2*G1 + g1
    + n3*N2*N1*N0 + n2*N1*N0 + n1*N0 + n0
    + m3*M2*M1*M0 + m2*M1*M0 + m1*M0 + m0
    + e0
    + f3*F2*F1*F0 + f2*F1*F0 + f1*F0 + f0;
inx_i = g3*G2*G1 + g2*G1 + g1
    + n3*N2*N1*N0 + n2*N1*N0 + n1*N0 + n0
    + e0
    + f3*F2*F1*F0 + f2*F1*F0 + f1*F0 + f0
    + c2*C1*C0 + c1*C0 + c0
    + r0
    + s2*S1 + s1;
inx_w = g3*G2*G1 + g2*G1 + g1
    + m3*M2*M1*M0 + m2*M1*M0 + m1*M0 + m0
    + c2*C1*C0 + c1*C0 + c0
    + r0
    + s2*S1 + s1;

inx_o[0] = m0 + e0*M0 + n0*E0*M0 + f0*N0*E0*M0;
inx_w[0] = m0 + c0*M0 + r0*C0*M0;
inx_i[0] = c0 + r0*C0 + e0*R0*C0 + n0*E0*R0*C0 + f0*N0*E0*R0*C0;
```



### Flexible and Scalable NoC

A new NoC named *`hierarchical mesh`* is designed to adapt to a wide range of bandwidth requirements.

#### The Pros and Cons of Different NoC Implementations

+ Broadcast Network: high reuse but low bandwidth
+ Unicast Network: high bandwidth but low reuse
+ All-to-All Network: implementation cost and energy consumption increase quadratically 

![The pros and cons of different NoC implementations](https://images-cdn.shimo.im/otl1ciltLJkHZZql__thumbnail)

#### Architecture and Work Models of Hierarchical Mesh Network

![Architecture of Hierarchical Mesh Network](https://images-cdn.shimo.im/KGUO8tm73RYl12vk/image.png__thumbnail)

![Work Models of Hierarchical Mesh Network](https://images-cdn.shimo.im/xxbVBVUFHdkow9E3/image.png__thumbnail)

#### Considerations

The size of clusters:

+ what is the bandwidth required from the source cluster in the worst case, i.e., unicast mode;
+ what is the bandwidth required in between the router clusters in the worst case, i.e., interleaved multicast mode;
+ what is the tolerable cost of the all-to-all network;

The tile size of the mapped data dimensions:

+ the cluster size
+ the number of clusters

## Eyeriss: A Spatial Architecture for Energy-Efficient Dataflow for Convolutional Neural Networks

Compared to the [Eyeriss v2](#eyeriss-v2-a-flexible-and-high-performance-accelerator-for-emerging-deep-neural-networks), this article provides a more detailed explanation of *Row Stationary*, a baseline storage area for a given number of PEs and the energy cost estimation for RS reuse pattern.

---

This article proposed RS dataflow which can adapt to different CNN shape configurations and reduces all types of data movement through maximally utilizing the processing engine (PE) local storage, direct inter-PE communication and spatial parallelism.

Also, an analysis framework that compares energy cost under the same hardware area and processing parallelism constraints.

### Framework for Energy Efficiency Analysis

#### Storage Area

The baseline storage area for a given number of PEs is calculated as

```
#PE×Area(512B RF)+Area((#PE×512B) global buffer).
```

![](https://images-cdn.shimo.im/H6Ir275vy2cj9SXP/image.png)

#### Input Data Access Energy Cost

Input data access energy cost estimation:

```
a×EC(DRAM)+ab×EC(global buffer)+abc×EC(array)+abcd×EC(RF)
```

![Example of input data](https://images-cdn.shimo.im/4HIcPqN7ZJYtzTZu/image.png)

#### Psum Accumulation Energy Cost

Psum accumulation energy cost estimation:

```
(2a−1)×EC(DRAM)+2a(b−1)×EC(global buffer)+ab(c−1)×EC(array)+2abc(d−1)×EC(RF)
```

![Example of Psum accumulation](https://images-cdn.shimo.im/YpjPqAj7BIkUqJOS/image.png)

where the `EC(*)` is shown as follows:

![NORMALIZED ENERGY COST RELATIVE TO A MAC OPERATION EXTRACTED FROM A COMMERCIAL 65NM PROCESS](https://images-cdn.shimo.im/QQxUYwRr9kEkrpyE/image.png)



## Eyeriss: An Energy-Efficient Reconfigurable Accelerator for Deep Convolutional Neural Networks

Compared to the [Eyeriss v2](#eyeriss-v2-a-flexible-and-high-performance-accelerator-for-emerging-deep-neural-networks) and [Eyeriss energy](#eyeriss-an-energy-efficient-reconfigurable-accelerator-for-deep-convolutional-neural-networks), this article provides a more detailed explanation on the system architecture of the NoC communication as well as mapping of the PE sets. Also, it introduces an RLC compressed form to reduce the data size as related components in PE architecture.

Plus, this article mentions CNN biases, but there is nothing related to the biases in the architecture it describes.

---

This article introduces the way to reduce the cost of data movement by exploiting data reuse in a multilevel memory hierarchy.  Also, techniques such as compression and data-adaptive processing are introduced to save both memory bandwidth and processing power.

### Main Features of Eyeriss

+ A four-level memory hierarchy. Data movement can exploit low-cost levels:
	+ the PE scratch pads
	+ the inter-PE communication
	+ the large on-chip global buffer (GLB)
	+ the off-chip DRAM.
+ A CNN dataflow, called Row Stationary (RS), that reconfigures the spatial architecture to map the computation of a given CNN shape and optimize for the best energy efficiency.
+ A network-on-chip (NoC) architecture that uses both multicast and point-to-point single-cycle data delivery to support the RS dataflow.
+ Run-length compression (RLC) and PE data gating that exploits the statistics of zero data in CNNs to further improve energy efficiency.

This is the architecture of Eyeriss system:

![Eyeriss system architecture](https://images-cdn.shimo.im/KUn0Ny1sa6wMuMuy/image.png)

### System Control and Configuration

The accelerator has two levels of control hierarchy. 

+ The top-level control coordinates:
	+ traffic between the off-chip DRAM and the GLB through the asynchronous interface;
	+ traffic between the GLB and the PE array through the NoC;
	+ operation of the RLC CODEC and ReLU module. 
+ The lower-level control consists of control logic in each PE, which runs independently of each other.

The size:

+ spad capacity depends only on the filter row size (S)
	but not the ifmap row size (W), and is equal to:
	+ S for a row of filter weights; 
	+ S for a sliding window of ifmap values;
	+ 1 for the psum accumulation.

+ the height and the width of the PE set are equal to the number of filter rows (R) and ofmap rows (E), respectively.

![Mapping of the PE sets on the spatial array of 168 PEs for the CONV layers in AlexNet](https://images-cdn.shimo.im/JlEPRdIC1rU50uoT/image.png)

### Exploit Data Statistics

data statistics of CNN is explored to:

+ reduce DRAM accesses using compression, which is the most energy-consuming data movement per access, on top of the optimized dataflow;
+ skip the unnecessary computations to save processing power

#### RLC

RLC is used in Eyeriss to exploit the zeros in fmaps and save DRAM bandwidth.

![an example of RLC encoding](https://uploader.shimo.im/f/Ir9k6AuBWP0NWomF.png!thumbnail)

#### NoC

Requirements of the NoC architecture:

+ support the data delivery patterns used in the RS dataflow; should be taken care of:
	+ different convolution strides (U) result in the ifmap delivery, skipping certain rows in the array;
	+ a set is divided into segments that are mapped onto different parts of the PE array;
	+ multiple sets are mapped onto the array simultaneously and different data is required for each set
+ leverage the data reuse achieved by the RS dataflow to further improve energy efficiency;
+ provide enough bandwidth for data delivery in order to support the highly parallel processing in the PE array.

Three different types of networks:

+ Global Input Network: is optimized for a single-cycle multicast from the GLB to a group of PEs that receive the same filter weight, ifmap value, or psum. The challenge is that the group of destination PEs varies across layers due to the differences in data type, convolution stride, and mapping.
+ Global Output Network: The GON is used to read the psums generated by a processing pass from the PE array back to the GLB.
+ Local Network: pass the psums from two consecutive rows of the same column

![Architecture of the GIN](https://images-cdn.shimo.im/RrYcTIHJCF4DOYXc/image.png)

#### Processing Element and Data Gating

The datapath is pipelined into three stages: one stage for spad access, and the remaining two for computation.

An extra 12-b Zero Buffer is used to record the position of zeros in the ifmap spad. If a zero ifmap value is detected from the zero buffer, the gating logic will disable the read of the filter spad and prevent the MAC datapath from switching.

![PE architecture](https://images-cdn.shimo.im/MPBbaUOFLNEcqibp/image.png)

## Some Troubles

### Scalac_2.12.4:4.2.3

I tried run verilator simulation with default command

```bash
make CONFIG=SmallBoomAndRocketConfig BINARY=mt-vvadd.riscv run-binary
```

But got the following error:

```
[warn] 	Note: Unresolved dependencies path:
[error] sbt.librarymanagement.ResolveException: Error downloading org.scalameta:semanticdb-scalac_2.12.4:4.2.3
[error]   Not found
[error]   Not found
[error]   not found: /home/singularity/.ivy2/local/org.scalameta/semanticdb-scalac_2.12.4/4.2.3/ivys/ivy.xml
[error]   not found: https://repo1.maven.org/maven2/org/scalameta/semanticdb-scalac_2.12.4/4.2.3/semanticdb-scalac_2.12.4-4.2.3.pom
[error]   not found: https://oss.sonatype.org/content/repositories/snapshots/org/scalameta/semanticdb-scalac_2.12.4/4.2.3/semanticdb-scalac_2.12.4-4.2.3.pom
[error]   not found: https://oss.sonatype.org/content/repositories/releases/org/scalameta/semanticdb-scalac_2.12.4/4.2.3/semanticdb-scalac_2.12.4-4.2.3.pom
```

When I got into the website it mentioned `https://repo1.maven.org/maven2/org/scalameta/` I could only found the version `4.2.3` in `https://repo1.maven.org/maven2/org/scalameta/semanticdb-scalac_2.13.1/4.2.3/`.

Firstly, I tried to change the Scala version to `2.13.1` but occurred some syntax errors when I tried to `sbt publishLocal` for `firrtl`, so I had to give up that way.

Then, I wanted to find why the default Scala version is `2.12.4` though I had set that to `2.12.1` in `build.sbt`. Then I found the default set was in `variables.mk:143`, so I changed it to `2.12.10`, which is the same to that in `firrtl`.

### No found: firrlt object

After that, I countered with the old trouble: `not found: object firrtl`

```
[error] /home/singularity/chipyard/tools/chisel3/src/main/scala/chisel3/ChiselExecutionOptions.scala:7:8: not found: object firrtl
[error] import firrtl.{AnnotationSeq, ExecutionOptionsManager, ComposableOptions}
[error]        ^
[error] /home/singularity/chipyard/tools/chisel3/src/main/scala/chisel3/ChiselExecutionOptions.scala:21:44: not found: type ComposableOptions
```

I tried to re-do `publishLocal` in almost all components in `chipyard/tools` but failed.

Then, Jiuyang told me that I was supposed to `sbt publishLocal` then `sbt update` in these three directories in order:

+ firrtl
+ chisel3
+ rocket-chip

But while I tried to `publishLocal` under rocket-chip, I found the version of firrtl it need was 1.2 while I only had 1.3 version. So I re-do this flow with a correctness of `version := "1.2-SNAPSHOT"`  in `build.sbt` under firrtl.

Although I published the rocket-chip, I could not build chipyard successfully.


# 2019/12/16-22

Last week, I cost Monday to review the growth in the last two weeks, and Tuesday to prepare for the presentation on Thursday. And also, I improved the program of matrix sum I wrote two weeks ago, with an additional C test file. I learned the basic Scala knowledge on Wednesday but still lack some practice. Thursday to Saturday, I followed Shien's advice to watch the tutorial provided by one team in MIT who creates the Eyeriss project. A module of one translation project related to Chisel, named `Bootcamp`, also was done by me.

Unfortunately, due to the complex and chaos steps of both Rocket Core and FPGA, which need to boot Linux, I haven't done the hardware simulation. However, after the monthly report to my advisor in UESTC, I got a more simple method to skip the boot, I'd try after Shiqing came back.

So this week, I'll focus on some papers mentioned in the tutorial, including [reference](https://github.com/SingularityKChen/Weekly_Review_in_NTU/blob/master/Reference.md) 1-5, and several papers Shien suggested me to read, including [reference](https://github.com/SingularityKChen/Weekly_Review_in_NTU/blob/master/Reference.md)  6-14.

---

## Functional Programming Principles in Scala

### Week1

#### Classes, Traits, Objects and Packages

##### Classes

Like in Java, classes can be instantiated using the new construct, there can be many “instances” (or “objects”) of the same class. Classes in Scala cannot have static members. You can use objects (see below) to achieve similar functionality as with static members in Java.

##### Traits

Traits are like interfaces in Java, but they can also contain concrete members, i.e. method implementations or field definitions.

##### Objects

The Object in Scala are like classes, but for every object definition, there is only one single instance.

##### Packages

Adding a statement such as package `foo.bar` at the top of a file makes the code in a file part of the package `foo.bar`. If you define a class `MyClass` in package `foo.bar`, you can import that specific class (and not anything else from that package) with import  `foo.bar.MyClass`.

##### Source Files

Package hierarchies should be reflected in the directory structure: a source file defining class C in package `foo.bar` should be stored in a  subdirectory as `foo/bar/C.scala`. Scala does not really enforce this convention, but some tools such as the Scala IDE for eclipse might have problems otherwise.

![coursera scala programming_and_computer](https://images-cdn.shimo.im/ityNPwFlFlYFAd2Y/coursera_scala_programming_and_computer.png)

![coursera_programming_theory](https://images-cdn.shimo.im/yMVPdcKxLiAcjY6p/coursera_programming_theory.png)

#### Coursera Elements of Programming

![coursera_elements_of_programming](https://uploader.shimo.im/f/adsbxh8TmaoV2Tdx.png!thumbnail)

In Scala, usually `call-by-value`, but can use `=>` to use `call-by-name`.

If `call-by-value`, then it will calculate the values of the arguments firstly.

#### Blocks

`{}`

The definitions inside a block are only visible from within the block

It's not just named space control but it's also reusing outer definitions without passing them explicitly in parameters. 

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

Last week, I finished the draft version of the matrix sum with Chisel and became half-familiar with it. Also, I tried to write a functional test with `Chisel-tester2`. Unfortunately, due to the complex and unstable environment, I can not check all the syntax or run the functional test, but I tried to use the environment in `Jupyter` to run a simple test and two small submodules had been proved right. In order to use Chisel and `Chipyard`'s components, I studied one sample project named [`SHA3`](#SHA3). If I have more time, I'd back here to restudy that.

Fortunately, while I was programming the matrix sum project, I realized the feeling of distinguishing the difference of Scala types and hardware types. I mean, I knew why we can utilize the features of Scala to boost our efficiency of design our circuits and when we should create a hardware register to store our mediate data, why the hardware type `UInt` can not be translated to the Scala type `Int`. Just because Scala is the hardware generator, it's a software programming language, its types exist when we build the project. But as for hardware types, they only exist when the circuits running. So we can not get an unknown value and translate it to a must-know value.

So this week, I'll try to finish the related C files and run the simulation on FPGA boards. Also, I need to learn some Scala programming skills to enhance my Chisel coding ability.

---

## Git Command
[some useful command and the explanation of git](https://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html).
![the work flow](https://www.ruanyifeng.com/blogimg/asset/2015/bg2015120901.png)

### the workflow

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

`git log` to show the current branch's changelog

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

`asm volatile` is the inline assembly that used in language Linux C. `asm` is the keyword of `gcc` and `volatile` means `gcc` don't need to optimize the assemble codes. There is [a brief introduction](https://blog.csdn.net/slvher/article/details/8864996) of this. And I found this term while I was trying to [test the custom extensive instructions](#Instruction test with C) with `RoCC` interface. This syntax can run our own instruction without change the compiler. Plus, it can help our C program to run several commands in order with the help of [`asm volatile("" ::: "memory")`](https://blog.csdn.net/KISSMonX/article/details/9105823).

This syntax also has a feature named [Atomic](https://blog.csdn.net/wudiyi815/article/details/8468745). That means this command only has to states in runtime: either success or failure.

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


// define the struct, need a pointer. m means row and n means column
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

If one wants to harness `RoCC` interface to add extensive instructions, he must design his Chisel hardware project firstly and then write some C tests to prove this project works. Then modify some C files to let the compiler utilize your new instructions.

#### `SHA3`-Chisel

##### `sha3.scala`

This is the top module of `SHA3` project.

![RoCCinSHA3](https://images-cdn.shimo.im/ErYI4gMr0rkyBzVu/RoCCinSHA3.png)

##### `ctrl.scala`

This is the most complex submodule of `SHA3` project. It contains how to decode the instructions, as well as three finite-state machines. One (`rocc_s`) shows the states of the communication between the control module and the CPU(or receives instructions from CPU and uses ready-valid signals to show some states of the accelerator); One (`mem_s`) shows the states of the communication between the control module and the memory(or receives data from memory and send data back to memory and also ready-valid signals); The last one (`state`) shows the states of the communication between the control module and its submodules.

`dpath.scala`

This is the data path of the whole module.

`dmem.scala`

This is the module responsible for transform data between memory and the accelerator. So we can use this module with just a slight modifying.

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

We can see it in more details from the photo below. **NOTE: this graph is a little old as it was published in 2015, and there are several slight changes.**

![RoCC 2015](https://images-cdn.shimo.im/ZhQo3Zs8BIQ8xrbL/RoCC2015.png)

### extension instruction with `RoCC`

The table below shows the `RoCC` instruction format.

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

With the help of TLB, the CPU may access the physical address [with just one accessment](https://blog.csdn.net/leishangwen/article/details/27190959). And it contains both some copies of the page table and the physical address.

#### TLB passthrough

When I looked through the `TLB.scala` I found a valuable named `passthrough`

### Cache Hierarchy

I found the following information when I tried to understand the meaning of `memory tag`. I met this term while I was trying to mimic [the control module](#SHA3-Chisel) of `SHA3`. Because it extends from [`HellaCache`](#HellaCache), which require a tag. Although finally, I found I can give out tag casually, I learned a lot from the structure of cache including TLB.

![Cache hierarchy of the K8 core in the AMD Athlon 64 CPU](https://pic3.zhimg.com/80/v2-0e77de9fe46e80c179204da1bf9ad6b2_hd.jpg)

The picture above illustrates the abstract architecture of the cache hierarchy of one AMD CPU, from which we can see [`TLB`](#TLB) and Cache. Both these two components contain two levels and save instructions and data separately.

If we see memory hierarchy excluding TLB, then it looks like the following picture:

![Cortex-A53 memory hierarchy](http://www-x-wowotech-x-net.img.abc188.com/content/uploadfile/201905/95101556803041.png)

And because of the dramatically booming speeds of high-level caches, they can improve the performance of the CPU. [We can regard](http://www.wowotech.net/memory_management/458.html) the cache is a method to improve the R/W speed of main memory, the CPU register is a method to improve that of the cache.

![cache speed](http://www-x-wowotech-x-net.img.abc188.com/content/uploadfile/201905/3d2f1556803040.jpg)

So how can the cache controller know whether the data we need is included in current cache? So `tag` comes.

![the usage of tag](http://www-x-wowotech-x-net.img.abc188.com/content/uploadfile/201906/914f1559459886.png)

We can see from the graph above, there is a tag array and its corresponding data array. Firstly, the controller will use the index to find a cache line and then compares the tag array, then bingo.

[Sinaean Dean](https://zhuanlan.zhihu.com/p/31875174) introduced the two usages of cache tag:

+ For caches parallelly connected together, using tags as a selection signal in a MUX to choose the right data.

+ Know whether cache hits or misses.

If the cache miss, then it will ask a lower lever cache for the data or instructions.

![cooperation between different levels of caches](http://www-x-wowotech-x-net.img.abc188.com/content/uploadfile/201905/560d1556803043.png)

### Memory Read (Synchronous and Asynchronous)
[This blog](https://www.cnblogs.com/kalo1111/p/3245056.html) describes the [synchronous memory](#Chisel3 Syntax) -- SSRAM, which is controlled by the clock and can not read or write at the same clock cycle. But if we use asynchronous memory, then we can get the read data within one clock and if we use a combinational/asynchronous-read, sequential/synchronous-write memory, then the read-after-write hazards [are not an issue](https://www.chisel-lang.org/api/latest/chisel3/Mem.html).

## Some Trouble

### `psutil`

ERROR INFO: `No module named psutil`.

But I have installed this module. Then via [some blogs](https://blog.csdn.net/weixin_42348333/article/details/85068701), I find that might be the version of python that excursive doesn't install the module. So I [changed the default version of python](https://blog.csdn.net/u011534057/article/details/51615193) from 2.7 to 3.5 and reinstall it with `pip3`.

### Import classes into `IntelliJ IDEA`

I use some modules of `chipyard`, which means I have to import that project into my project.

In the beginning, I just add the files which contain the Scala files directly, but it didn't work at all. Because the packages and classes are defined at a more high level.

By accidentally, I included the highest file (`generator` in this case) and it succeeds.

### Import reliance into `sbt` project

```scala
lazy val matrixsum = conditionalDependsOn(project in file("/home/singularity/chipyard/generators/matrixsum"))
		.dependsOn(rocketchip, chisel_testers, sifive_blocks, sifive_cache, utilities, midasTargetUtils)
		.settings(commonSettings)
```

But that's works on the top module, the `dependson` must be a subfile of the current `build.sbt`  directory.

### Can not find the lvy local lib

~~Haven't solved yet~~

---

Solved at Dec 28th by republish it with the value of `version := 1.2-SNAPSHOT` in `build.sbt`.

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
