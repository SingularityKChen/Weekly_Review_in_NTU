# Table of Contents

<!-- MarkdownTOC -->

1. [2019/12/30-2020/01/05](#20191230-20200105)
	1. [A Survey on Methods and Theories of Quantized Neural Networks](#a-survey-on-methods-and-theories-of-quantized-neural-networks)
		1. [Quantization Techniques](#quantization-techniques)
		1. [Two Quantization Methodologies](#two-quantization-methodologies)
		1. [Discussion](#discussion)
		1. [Why Does QuantizationWork?](#why-does-quantizationwork)
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

# 2019/12/30-2020/01/05

Last week, I only read one article which is the survey of QNN. Although I know how to utilize several QNN models to reduce the memory requirements and power consumption, I don't understand why those formulas works  due to the lack of some basic mathematic knowledge in neural network.

I took my main time and energy to write my initial stage's report of my final year's project. During that period, I recall the paper I read the past month and thought over my project in a more detail. I though I could start my project with the basic processing element and then read more paper to find something interesting and then optimize the design.

Also, before the new year, I created one new Linux virtual machine with Virtual Box. And after new year, I downloaded the latest Chipyard project from GitHub and rebuilt the environment in both mine virtual machine and Shien's. Fortunately, the new environment works now.

And this week, I will try to read as many Song Han's papers as possible before his seminar, and then I will read some other papers [listed before](#20191216-22). I hope I could finish the translation work of chisel-bootcamp module 3.6 before this Saturday. After that, I will begin the design of PE in my FYP.

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

### Why Does QuantizationWork?

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

So this week, I'll focus on some papers mentioned in the tutorial, including bibliography 1-5, and several papers Shien suggested me to read, including bibliography  6-14.

- [x] \[1\] V. Sze, Y.-H. Chen, T.-J. Yang, and J. S. Emer, “Efficient Processing of Deep Neural Networks: A Tutorial and Survey,” *Proceedings of the IEEE*, vol. 105, no. 12, pp. 2295–2329, Dec. 2017, doi: [10.1109/JPROC.2017.2761740](https://doi.org/10.1109/JPROC.2017.2761740).

- [x] \[2\] Y.-H. Chen, T.-J. Yang, J. Emer, and V. Sze, “Eyeriss v2: A Flexible Accelerator for Emerging Deep Neural Networks on Mobile Devices,” *arXiv:1807.07928 [cs]*, May 2019.

- [x] \[3\] Y.-H. Chen, J. Emer, V. Sze, Y.-H. Chen, J. Emer, and V. Sze, “Eyeriss: a spatial architecture for energy-efficient dataflow for convolutional neural networks,” *ACM SIGARCH Computer Architecture News*, vol. 44, no. 3, pp. 367–379, Jun. 2016, doi: [10.1109/ISCA.2016.40](https://doi.org/10.1109/ISCA.2016.40).

- [x] \[4\]  Y.-H. Chen, T. Krishna, J. S. Emer, and V. Sze, “Eyeriss: An Energy-Efficient Reconfigurable Accelerator for Deep Convolutional Neural Networks,” *IEEE J. Solid-State Circuits*, vol. 52, no. 1, pp. 127–138, Jan. 2017, doi: [10.1109/JSSC.2016.2616357](https://doi.org/10.1109/JSSC.2016.2616357).

- [ ] \[5\] V. Sze, Y.-H. Chen, J. Emer, A. Suleiman, and Z. Zhang, “Hardware for Machine Learning: Challenges and Opportunities,” *2017 IEEE Custom Integrated Circuits Conference (CICC)*, pp. 1–8, Apr. 2017, doi: [10.1109/CICC.2017.7993626](https://doi.org/10.1109/CICC.2017.7993626).

- [x] \[6\] Y. Guo, “A Survey on Methods and Theories of Quantized Neural Networks,” *arXiv:1808.04752 [cs, stat]*, Dec. 2018.

- [ ] \[7\] S. Zheng, Y. Liu, S. Yin, L. Liu, and S. Wei, “An efficient kernel transformation architecture for binary- and ternary-weight neural network inference,” presented at the Proceedings of the 55th Annual Design Automation Conference, 2018, p. 137, doi: [10.1145/3195970.3195988](https://doi.org/10.1145/3195970.3195988).

- [ ] \[8\] Y. Hu *et al.*, “BitFlow: Exploiting Vector Parallelism for Binary Neural Networks on CPU,” in *2018 IEEE International Parallel and Distributed Processing Symposium (IPDPS)*, 2018, pp. 244–253, doi: [10.1109/IPDPS.2018.00034](https://doi.org/10.1109/IPDPS.2018.00034).

- [ ] \[9\] Y. Chen *et al.*, “DaDianNao: A Machine-Learning Supercomputer,” in *2014 47th Annual IEEE/ACM International Symposium on Microarchitecture*, Cambridge, United Kingdom, 2014, pp. 609–622, doi: [10.1109/MICRO.2014.58](https://doi.org/10.1109/MICRO.2014.58).

- [ ] \[10\] Y. Chen, T. Chen, Z. Xu, N. Sun, and O. Temam, “DianNao family: energy-efficient hardware accelerators for machine learning,” *Commun. ACM*, vol. 59, no. 11, pp. 105–112, Oct. 2016, doi: [10.1145/2996864](https://doi.org/10.1145/2996864).

- [ ] \[11\] T. Chen *et al.*, “DianNao: a small-footprint high-throughput accelerator for ubiquitous machine-learning,” in *Proceedings of the 19th international conference on Architectural support for programming languages and operating systems - ASPLOS ’14*, Salt Lake City, Utah, USA, 2014, pp. 269–284, doi: [10.1145/2541940.2541967](https://doi.org/10.1145/2541940.2541967).

- [ ] \[12\] S. Han *et al.*, “EIE: Efficient Inference Engine on Compressed Deep Neural Network,” *arXiv:1602.01528 [cs]*, May 2016.

- [ ] \[13\] N. P. Jouppi *et al.*, “In-Datacenter Performance Analysis of a Tensor Processing UnitTM,” p. 17.

- [ ] \[14\] Z. Du *et al.*, “ShiDianNao: shifting vision processing closer to the sensor,” in *Proceedings of the 42nd Annual International Symposium on Computer Architecture - ISCA ’15*, Portland, Oregon, 2015, pp. 92–104, doi: [10.1145/2749469.2750389](https://doi.org/10.1145/2749469.2750389).


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
