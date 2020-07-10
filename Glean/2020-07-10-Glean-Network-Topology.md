---
layout: post
title: "[Glean] Network Topology"
description: "Network topology is the arrangement of the elements of a communication network. Including point to point, bus, star, ring or circular, mesh, tree, hybrid, or daisy chain."
categories: [Glean]
tags: [Mesh, Network]
last_updated: 2020-07-10 20:20:00 GMT+8
excerpt: "Network topology is the arrangement of the elements of a communication network. Including point to point, bus, star, ring or circular, mesh, tree, hybrid, or daisy chain."
redirect_from:
  - /2020/07/10/
---

* Kramdown table of contents
{:toc .toc}
# Network Topology[^1]

Network topology is the arrangement of the elements (links, nodes, etc.) of a communication network.

The study of network topology recognizes eight basic topologies: point-to-point, bus, star, ring or circular, mesh, tree, hybrid, or daisy chain.

## Point-to-point

point-to-point topology, is a point-to-point communication channel that appears, to the user, to be permanently associated with the two endpoints.

## Daisy chain

Daisy chaining is accomplished by connecting each computer in series to the next.

A daisy-chained network can take two basic forms: linear and ring. 

![Daisy Chain](https://upload.wikimedia.org/wikipedia/commons/thumb/c/cf/Daisy_chain.svg/220px-Daisy_chain.svg.png)

## Bus

In local area networks using bus topology, each node is connected by interface connectors to a single central cable. This is the 'bus', also referred to as the backbone, or trunk) – all data transmitted between nodes in the network is transmitted over this common transmission medium and is able to be received by all nodes in the network simultaneously.



![Bus](https://upload.wikimedia.org/wikipedia/commons/thumb/4/47/BusNetwork.svg/220px-BusNetwork.svg.png)

### Linear bus

In a linear bus network, all of the nodes of the network are connected to a common transmission medium which has just two endpoints.

### Distributed bus

In a distributed bus network, all of the nodes of the network are connected to a common transmission medium with more than two endpoints, created by adding branches to the main section of the transmission medium.

## Star

In star topology, every peripheral node (computer workstation or any  other peripheral) is connected to a central node called a hub or switch. The hub is the server and the peripherals are the clients.

![Star](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d0/StarNetwork.svg/220px-StarNetwork.svg.png)

### Extended star

The extended star network topology extends a physical star topology by one or more repeaters between the central node and the peripheral (or 'spoke') nodes.

### Distributed star

A distributed star is a network topology that is composed of individual  networks that are based upon the physical star topology connected in a  linear fashion – i.e., 'daisy-chained' – with no central or top level  connection point.

## Ring

A ring topology is a bus topology in a closed loop. Data travels around the ring in one direction. When one node sends data to another, the data passes through each intermediate node on the ring until it reaches its destination.

Advantages:

+ When the load on the network increases, its performance is better than bus topology.
+ There is no need of network server to control the connectivity between workstations.

Disadvantages:

+ Aggregate network bandwidth is bottlenecked by the weakest link between two nodes.

![Ring](https://upload.wikimedia.org/wikipedia/commons/thumb/7/75/RingNetwork.svg/220px-RingNetwork.svg.png)

## Mesh

The value of fully meshed networks is proportional to the exponent of the number of subscribers, assuming that communicating groups of any two endpoints, up to and including all the endpoints, is approximated by Reed's Law. 

### Fully connected network

In a *fully connected network*, all nodes are interconnected.

![Mesh](https://upload.wikimedia.org/wikipedia/commons/thumb/3/3c/NetworkTopology-FullyConnected.png/220px-NetworkTopology-FullyConnected.png)



### Partially connected network

In a partially connected network, certain nodes are connected to exactly one other node; but some nodes are connected to two or more other nodes with a point-to-point link.

![Hybrid](https://upload.wikimedia.org/wikipedia/commons/thumb/9/97/NetworkTopology-Mesh.svg/220px-NetworkTopology-Mesh.svg.png)

## Hybrid

Hybrid topology is also known as hybrid network. Hybrid networks combine two or more topologies in such a way that the resulting network does not exhibit one of the standard topologies (e.g., bus, star, ring, etc.). For example, a tree network (or star-bus network) is a hybrid topology in which star networks are interconnected via bus networks.

[^1]: Network topology, Wikipedia. https://en.wikipedia.org/wiki/Network_topology.