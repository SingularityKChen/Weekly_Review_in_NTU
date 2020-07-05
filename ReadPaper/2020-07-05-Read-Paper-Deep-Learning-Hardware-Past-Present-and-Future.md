---
layout: post
title: "[Read Paper] Deep Learning Hardware: Past, Present, and Future"
description: "Reading note of Deep Learning Hardware: Past, Present, and Future"
categories: [ReadPaper]
tags: [Survey, DLA]
last_updated: 2020-07-05 10:02:26 GMT+8
excerpt: "This paper introduce the following aspects: 1) identifies trends in deep learning research that will influence hardware architectures and software platforms of the future; 2) Five DL use cases with different hardware requirements; 3) Present and Future Deep-Learning Architectures; 4) Requirements for Future DL Hardware and Software"
redirect_from:
  - /2020/07/05/
---

* Kramdown table of contents
{:toc .toc}
# Deep Learning Hardware: Past, Present, and Future

This paper introduce the following aspects:

+ identifies trends in deep learning research that will influence hardware architectures and software platforms of the future
+ Five DL use cases with different hardware requirements
+ Present and Future Deep-Learning Architectures
+ Requirements for Future DL Hardware and Software

## Past

### Lose popularity in the mid-1990s

What caused it to lose popularity within the research community in the mid-1990s?

+ The performance of computers at the time;
+ The small number of applications for which collecting large labeled datasets was cost effective;
+ The effort involved in developing flexible neural net simulators; 
+ The reluctance of many research institutions at the time to distribute open source software. 

### Sudden Resurgence around 2013

What sparked its sudden resurgence around 2013? There are four main factors: 

+ Improved methods; 
+ Larger datasets with many samples and many categories;
+ Low-cost TFLOPS-class general-purpose GPUs (GPGPUs);
+ Open-source libraries with interpreted language frontends (Torch, Theano, cuda-convNet, Caffe). 
+ The first three of these enabled record-breaking results in image recognition and speech recognition;
+ The last one allowed these results to be easily replicated because they incorporated all the engineering “tricks” necessary to get DL models to work.

### The success of GPGPU for DL

What has accounted for the success of GPGPU for DL

+ Wide availability
+ Generality
+ Programmability
+ well-supported software stacks

### The unsuccess of dedicated hardware architectures 

Why none of dedicated hardware architectures for neural networks were successful

+ Lacked flexibility 
+ Were designed for particular types of neural networks that had no proven practical use

## The Need for DL Hardware: five use cases

The trend is to rely increasingly on unsupervised, self-supervised, weakly supervised or multi-task learning, for which larger networks perform even better.

There are five use cases with different hardware requirements.

### DL research and development

+ HPC type multi-node machines
+ The communication network must be **high bandwidth and low latency** to allow for the parallelization of training large models on large datasets
+ Using FP-32 is necessary because one must be sure that when an experiment fails, it is not because of a lack of **numerical accuracy**
+ The best architectures are those that can be saturated with the smallest batch of samples
+ Price and power consumption are relatively secondary to performance and flexibility

### Off-line training of DL models for production

+ Retraining with new data
+ It is possible to perform routine training on specialized hardware with **reduced-precision arithmetic**

### Inference on servers in data centers

+ Many are relatively “simple” neural networks **with sparse inputs**. But a lot of computation goes into **larger** **ConvNets**. 
+ Power consumption and cost are important, flexibility and raw performance are secondary, communication latency is unimportant.
+ The ideal architecture is a specialized DL-inference accelerator sitting in a standard data-center server node
+ Applications
  + Newsfeed, advertisement ranking, text classification
  + Image, video, and speech understanding, as well as for language translation

### Inference on mobile devices and embedded systems

+ DL-inference accelerators with very-low power consumption
+ Real-time tasks require that the DL system be run on the device without the latency of a round-trip to a server
+ Applications
  + feature tracking and 3D reconstruction for AR
  + object segmentation/recognition
  + OCR in natural scenes
  + real-time language translation
  + speech-based virtual assistants

### On-line learning on servers and mobile devices

### Typical Basic DL Modules

+ Multiple convolutions in 1D, 2D, and 3D; 
+ Linear operators (matrices); 
+ Linear operators applied to sparse inputs (word embedding lookup tables for NLP); 
+ Divisive normalization; 
+ Element-wise functions; 
+ Pooling/subsampling; 
+ Element-wise operators; 
+ Bilinear operators (multiplicative interactions for attention);

## Future

### Architectural Elements of Future DL Systems

#### Dynamic Networks, Differentiable Programming

+ The network architecture is dynamic and changes for every new data point. Needs back-propagates (autograd).
+ Applications
  + For natural-language processing, 
  + For data that does not come in the form of a fixed-sized tensor, 
  + For systems that need to activate parts of a large network on demand in a data-dependent way (such as the Multi-scale Densenet architecture)
  + For “reasoning” networks whose output is another network specifically designed to answer a particular question

#### Neural Networks on Graphs

+ Many problems are difficult to represent with fixed-size tensors or variable-length sequences of tensors, but are better represented by graphs whose **arcs and nodes are annotated by tensors**.
+ Convolution operations can easily be defined on irregular graphs: they are defined as **diagonal operators** in the eigenspace of the graph Laplacian, which is **a generalization of the Fourier transform**
+ Such networks for a wide variety of applications, which are likely to **violate the assumptions of current DL hardware**
+ Applications
  + 3D meshes, social networks, gene-regulation networks, and chemical molecules

#### Graph Embedding Networks

+ DL is used for largescale embedding of knowledge bases
+ Applications
  + Recommender systems
  + Use hyperbolic metric spaces to represent hierarchical categories

#### Memory-Augmented Networks

+ To endow DL systems with the ability to reason, they need a short-term memory, to be used as an episodic memory, or a scratchpad/working memory.
+ if a system is to answer questions about a series of events, it must be able to store the story in a memory and retrieve the relevant bits to answer a particular question

#### Complex Inference and Search

+ the output variable actually be an input to a scoring network whose scalar output (akin to energy) indicates the incompatibility between the input and an output proposal.
+ An inference procedure must search for the output value that minimizes the energy. This type of model is called an energy-based model.
+ If the energy minimizing inference procedure is gradient-based, inference hardware will need to support back-propagation.

#### Sparse Activations

+ The modules’ activations will become increasingly sparse, with only a subset of variables of a subset of modules being activated at any one time.
+ Need hardware support most neurons are quiet most of the time, which is good for power dissipation

#### Self-Supervised Learning (Generative Adversarial Networks)

### Requirements for Future DL Software

+ what is needed is a software framework for differentiable programming that is both interactive, flexible, dynamic, and efficient.
+ It is often impractical to develop high-volume applications or embedded applications that rely on Python at runtime.
  + For static compute graphs, there is no issue: one can export the graph to adhere to a standard format, such ONNX (Open Neural Net Exchange), and use one of the numerous ONNX-compliant backends.
  + For dynamic networks, there are two main options: One is to provide a compiler; another option is to design a suitable compilable language from scratch.

### Requirements for Future DL Hardware for Training

+ One problem is that sparsity, architecture dynamicity, and modules that manipulate non-tensor data (graphs), **break the assumption** that one can perform computation on batches of identically-sized samples.
  + Need new hardware architectures that can function efficiently **with a batch size of one**.
+ handling sparse structured data is another requirement.
  + When most units are off most of the time, it may become advantageous to make our hardware **event driven**, so that only the units that are activated consume resources

### Requirements for Future DL Hardware for Inference

+ require extremely low-power ASICs for DL inference for such things as real-time/low-latency object tracking, 3D re-construction, instance labelling, facial reconstruction, predictive compression and display.
+ the solution to power constraints may well be the exploitation of sparse activations, perhaps using event-based computation

