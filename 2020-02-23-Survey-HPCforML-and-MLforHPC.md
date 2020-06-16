---
layout: post
title: "[Survey] HPCforML and MLforHPC"
description: "the survey of HPCforML and MLforHPC"
categories: [ReadPaper, Survey]
tags: [ML4HPC, HPML, HPCforML, SystemforML, MLforSystem]
last_updated: 2020-03-07 10:32:00 GMT+8
excerpt: "This survey contains two papers 1) Understanding ML driven HPC: Applications and Infrastructure; 2) Learning Everywhere: A Taxonomy for the Integration of Machine Learning and Simulations."
redirect_from:
  - /2020/02/23/
---

* Kramdown table of contents
{:toc .toc}
# HPCforML and MLforHPC Survey

Due to the possibility of the population of `MLforHPC` and the next generation hybrid components architectures, we began the survey of `HPCforML` and `MLforHPC`, but I'd like to call it as `SystemforML` and `MLforSystem`, then we can use this kind of terms as a guidance to design our hybrid architectures not only in HPC but also other levels.

`HPCforML`, or `SystemforML`, uses HPC to execute and enhance ML performance, or using HPC simulations to train ML algorithms (theory guided machine learning), which are then used to understand experimental data or simulations.

`MLforHPC`, or `MLforSystem`,  uses ML to enhance HPC applications and systems, where big data comes from the computation and/or experimental sources that are coupled to ML and/or HPC elements. `MLforHPC` can be further subdivided as `MLaroundHPC`, `MLControl`, `MLAutotuningHPC`, and `MLafterHPC`.

## HPC and Big Data

### [HPC and Big Data (Long) – Part  A: Outline, Data on the Evolution of Interests and Communities](https://www.youtube.com/watch?v=HZsOqdx44yE)

### [HPC and Big Data (Long) – Part B: More on the Evolution of Interests and Communities](https://www.youtube.com/watch?v=AoE4QcG0PYE)

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200220195711.png)

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200220200123.png)

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200220200247.png)

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200220200311.png)

### [HPC and Big Data (Long)  – Part C: MLaroundHPDC/HPC and MLAutotuning](https://www.youtube.com/watch?v=Jz5t6h5wTPk)

MLAutotuning and MLaroundHPC

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200220200740.png)

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200220201043.png)

### [HPC and Big Data (Long)  – Part D: Learning Model Details and Agent Based Simulations](https://www.youtube.com/watch?v=azt-jwNKfa0)

### [HPC and Big Data (Long) – Part  E: Challenges and Opportunities, Conclusions](https://www.youtube.com/watch?v=xHrcSf19UTM)

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200220211517.png)

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200220211546.png)

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200220212307.png)

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200220212238.png)

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200220212402.png)



## Understanding ML driven HPC: Applications and Infrastructure

### HPCforML

`HPCforML` uses HPC to execute and enhance ML performance, or using HPC simulations to train ML algorithms (theory guided machine learning), which are then used to understand experimental data or simulations.

### MLforHPC

`MLforHPC` uses ML to enhance HPC applications and systems, where big data comes from the computation and/or experimental sources that are coupled to ML and/or HPC elements. `MLforHPC` can be further subdivided as `MLaroundHPC`, `MLControl`, `MLAutotuningHPC`, and `MLafterHPC`.

#### MLaroundHPC

Using ML to learn from simulations and produce learned surrogates for the simulations. This increases effective performance for strong scaling where we keep the problem fixed but run it faster or run the simulation for a longer time interval such as that relevant for biological systems. It includes `SimulationTrainedML` where the simulations are performed to directly train an AI system rather than the AI system being added to learn a simulation.

##### MLaroundHPC: Learning Outputs from Inputs

Simulations performed to directly train an AI system, rather than AI system being added to learn a simulation.

##### MLaroundHPC: Learning Simulation Behavior

ML learns behavior replacing detailed computations by ML surrogates.

##### MLaroundHPC: Faster and Accurate PDE Solutions

ML accelerated algorithms approximate the solution high dimensional PDEs such as the diffusion equation using a `Deep Galerkin Method (DGM)`, and train their network on batches of randomly sampled time and space points.

##### MLaroundHPC: New Approach to Multi-Scale Modeling

Machine learning is ideally suited for defining effective potentials and order parameter dynamics, and shows significant promise to deliver orders of magnitude performance increases over traditional coarse-graining and order parameter approaches. See well established methods.

#### MLControl

##### Experiment Control

Using simulations (possibly with HPC) in control of experiments and in objective driven computational campaigns.

##### Experiment Design

Model-based design of experiments (MBDOE) with new ML assistance [24] identifies the optimal conditions for stimuli and measurements that yield the most information about the system given practical limitations on realistic experiments.

#### MLAutoTuning

MLAutoTuning can be applied at multiple distinct points, and can be used for a range of tuning and optimization objectives. For example: 

+ mix of performance and quality of results using parameters provided by learning network [4], [25]–[28];
+ choose the best set of “computation defining parameters” to achieve some goal such as providing the most efficient training set with defining parameters spread well over the relevant phase space [29], [30];
+ tuning model parameters to optimize model outputs to available empirical data [31]–[34].

#### MLafterHPC

ML analyzing results of HPC as in trajectory analysis and structure identification in biomolecular simulations.

## Learning Everywhere: A Taxonomy for the Integration of Machine Learning and Simulations

![The 8 MLAutotuning and MLaroundHPC scenarios described in text](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200229111927.png)

### HPCforML

#### HPCrunsML

Using HPC to execute ML with high performance.

#### SimulationTrainedML

Using HPC simulations to train ML algorithms, which are then used to understand experimental data or simulations.

### Taxonomy Of MLAutotuningHPC And MLaroundHPC: Improving Simulation With Configurations And Integration Of Data

#### MLAutotuningHPC: Learn configurations

Figure 2 illustrates this category, which is classic `MLAutotuningHPC` and one optimizes some mix of performance and quality of results with the learning network inputting the configuration parameters of the computation. 

The configuration includes:

+ initial values
+ dynamic choices such as block sizes for cache use, variable step sizes in space and time. 

This category can also include discrete choices as to the type of solver to be used.

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200229113246%20MLAutotuningHPC%20%E2%80%93%20Learn%20configurations.png" alt="MLAutotuningHPC – Learn configurations" style="zoom:50%;" />

+ Particle Dynamics-MLAutotuningHPC – Learn configurations

#### MLAutotuningHPC: Learning Model Setups from Observational Data

This category is seen when a simulation set up as a set of agents, perhaps each representing a cell in a virtual tissue simulation. One can use ML to learn the dynamics of cells replacing detailed computations by ML surrogates. 

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200229113515MLAutoTuningHPC%3A%20Learning%20Model%20Setups%20from%20Observational%20Data.png" alt="MLAutoTuningHPC: Learning Model Setups from Observational Data" style="zoom: 67%;" />

+ Particle Dynamics-MLAutotuningHPC – Learning Model Setups from Observational Data
+ ABM-MLAutotuningHPC – Learning Model Setups from Observational Data
+ PDE-MLAutotuningHPC – Learning Model Setups from Observational Data

#### MLaroundHPC: Learning Model Details - ML for Data Assimilation (predictor-corrector approach)

Data assimilation involves continuous integration of time dependent simulations with observations to correct the model with a suitable combined data plus simulation model. This is for example common practice in weather prediction field. Such current state of the art expresses the spatial structure as a convolutional neural net and the time dependence as recurrent neural net (LSTM).

Then as a function of time one iterates a predictor corrector approach, where one time steps models and at each step optimize the parameters to minimize divergence between simulation and ground truth data.

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200229114144MLaroundHPC%3A%20Learning%20Model%20Details%20-%20ML%20for%20Data%20Assimilation%20(predictor-corrector%20approach).png" alt="MLaroundHPC: Learning Model Details - ML for Data Assimilation (predictor-corrector approach)" style="zoom:67%;" />

+ ABM-MLaroundHPC: Learning Model Details (ML based data assimilation)
+ PDE-MLaroundHPC: Learning Model Details (ML based data assimilation)

###  Taxonomy Of MLAutotuningHPC And MLaroundHPC: Learn Structure, Theory And Model For Simulation

#### MLAutotuningHPC: Smart ensembles

Here we choose the best strategy to achieve some computation goal such as providing the most efficient training set with defining parameters spread well over the relevant phase space. Ensembles are also essential in many computational studies such as weather forecasting or search for new drugs where regions of defining parameters need to be searched.

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200229114448MLAutotuningHPC%20%E2%80%93%20Smart%20ensembles.png" alt="MLAutotuningHPC – Smart ensembles" style="zoom:67%;" />

+ General Simulations-MLAutotuningHPC – Smart ensembles
+ Particle Dynamics-MLAutotuningHPC – Smart ensembles
+ ABM-MLAutotuningHPC – Smart ensembles

#### MLaroundHPC: Learning Model Details (effective potentials and coarse graining)

This is classic coarse graining strategy with recently, deep learning replacing dimension reduction techniques.) One can learn effective potentials and interaction graphs.

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200229154806MLaroundHPC%3A%20Learning%20Model%20Details%20(effective%20potentials%20and%20coarse%20graining).png" alt="MLaroundHPC: Learning Model Details (effective potentials and coarse graining)" style="zoom:67%;" />

+ Particle Dynamics-MLaroundHPC: Learning Model Details (effective potentials)
+ Particle Dynamics-MLaroundHPC: Learning Model Details (coarse graining)
+ PDE-MLaroundHPC: Learning Model Details (coarse graining)

#### MLaroundHPC: Learning Model Details - Inference of Missing Model Structure

AI will essentially be able to derive theories from data, or more practically a mix of data and models. This is especially promising in agent based models which often contain phenomenological approaches such as the predictor-corrector method.

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200229155054%20MLaroundHPC%3A%20Learning%20Model%20Details%20-%20Inference%20of%20Missing%20Model%20Structure.png" alt="MLaroundHPC: Learning Model Details - Inference of Missing Model Structure" style="zoom:67%;" />

###  Taxonomy Of MLAutotuningHPC And MLaroundHPC: Learn Surrogates For Simulation

#### MLaroundHPC: Learning Outputs from Inputs: a) Computation Results from Computation defining Parameters

In this category, one just feeds in a modest number of meta-parameters that define the problem and learn a modest number of calculated answers.

Operationally this category is the same as `SimulationTrainedML` but with a different goal: In `SimulationTrainedML` the simulations are performed to directly train an AI system rather than the case here where the AI system is being added to learn a simulation.

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200229155133MLaroundHPC%3A%20Learning%20Outputs%20from%20Inputs%3A%20Computation%20Results%20from%20Computation%20defining%20Parameters.png" alt="MLaroundHPC: Learning Outputs from Inputs: Computation Results from Computation defining Parameters" style="zoom:67%;" />

+ Particle Dynamics- MLaroundHPC: Learning Outputs from Inputs (parameters)
+ PDE-MLaroundHPC - Learning Outputs from Inputs (parameters)

#### MLaroundHPC: Learning Outputs from Inputs: b) Fields from Fields

Here one feeds in initial conditions and the neural network learns the result where initial and final results are fields.

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200229155522MLaroundHPC%3A%20Learning%20Outputs%20from%20Inputs%3A%20Fields%20from%20Fields.png" alt="MLaroundHPC: b) Learning Outputs from Inputs: Fields from Fields" style="zoom:67%;" />

+ Particle Dynamics-MLaroundHPC: Learning Outputs from Inputs (fields)
+ ABM-MLaroundHPC: Learning Outputs from Inputs (fields)
+ PDE-MLaroundHPC: Learning Outputs from Inputs (fields)



## [ML for HPC and HPC for AI Workflows: The Differences, Gaps and Opportunities with Data Management](https://www.sc-asia.org/2018/wp-content/uploads/2018/03/1_1800_Dr-Rangan-Sukumar.pdf)

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200216204306.png)

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200216201413.png)

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200216201441.png)

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200216201504.png)

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200216201602.png)



## Intelligent Data Center Architecture to Enable Next Generation HPC AI Platforms

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200216204531.png)

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200216204622.png)



## [Deep Learning on HPC: Performance Factors and Lessons Learned](http://www.benchcouncil.org/bench19/file/slides/invite4.pdf)

### Scale up vs. Scale out

![Scale up vs. Scala out](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200215195757.png)

### Conclusion

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200215200136.png)

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200215200240.png)