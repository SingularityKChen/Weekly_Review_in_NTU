---
layout: post
title: "[Tutorial] TileLink Spec"
description: "The study of SiFive TileLink"
categories: [Tutorial]
tags: [Chisel, TileLink]
last_updated: 2020-03-23 14:48:00 GMT+8
excerpt: "The study of SiFive TileLink. Including TileLink buses, nodes and its chisel codes in chipyard."
redirect_from:
  - /2020/03/21/
---

* Kramdown table of contents
{:toc .toc}
# TileLink Spec

[Newest English Version Spec](https://www.sifive.com/documentation/tilelink/tilelink-spec/), [Chinese Version](https://cnrv.io/assets/files/tilelink-spec-1.7.1-draft.zh.pdf).

## Protocol

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200329134935TileLinkFiveChannels.png" alt="TileLink Five Channels" style="zoom:67%;" />

## Buses and Nodes

### TileLink Buses

You can find the bus wrapper at `tilelink/BusWrapper.scala` and the definition of the buses under the file `subsystem/BaseSubsystem.scala:79`.

```scala
  val ibus = new InterruptBusWrapper()
  val sbus = LazyModule(p(BuildSystemBus)(p))
  val pbus = LazyModule(new PeripheryBus(p(PeripheryBusKey), "subsystem_pbus"))
  val fbus = LazyModule(new FrontBus(p(FrontBusKey)))
  val mbus = LazyModule(new MemoryBus(p(MemoryBusKey)))
  val cbus = LazyModule(new PeripheryBus(p(ControlBusKey), "subsystem_cbus"))
```

### TileLink Nodes

We can grep several kinds of nodes, such as `AXIAdapterNode`, `AXI4AsyncSourceNode`, `AXI4AsyncSinkNode`, `AXIToTLNode`, `AXI4AdapterNode`, `AXI4IdentityNode`, `AXI4RegisterNode`, `AHBFanoutNode`, `AHBMasterAdapterNode`, `AHBToTLNode`,  `TLToAPBNode`, `TLToAXI4Node`, `TLToAHBNode`, etc.

However, the source ones are defined at `tilelink/Node.scala` and you can find official introductions at `chipyard` documents [here](https://chipyard.readthedocs.io/en/latest/TileLink-Diplomacy-Reference/NodeTypes.html). 

#### `TLClientNode`

The client node is the master in TileLink spec.

>  It can send requests on the A channel and receive responses on the D channel. If the client implements TL-C, it will receive probes on the B channel, send releases on the C channel, and send grant acknowledgements on the E channel.
>
>  The L1 caches and DMA devices in RocketChip/Chipyard have client nodes.

##### Create a `TLCilentNode`

>  You can add a TileLink client node to your `LazyModule` using the `TLHelper` object from `testchipip` like so:
>
>  ```scala
>  class MyClient(implicit p: Parameters) extends LazyModule {
>    val node = TLHelper.makeClientNode(TLClientParameters(
>      name = "my-client",
>      sourceId = IdRange(0, 4),
>      requestFifo = true,
>      visibility = Seq(AddressSet(0x10000, 0xffff))))
>  
>    lazy val module = new LazyModuleImp(this) {
>      val (tl, edge) = node.out(0)
>  
>      // Rest of code here
>    }
>  }
>  ```
>  **But if you used the `TLHelper`, you only specified a single client edge**!

##### Arguments Needed for `TLClisentNode`

It only need a `TLClientParameters`, including:

+ `name`: identifies the node in the Diplomacy graph. It is the only required argument for `TLClientParameters`.
+ `sourceId`: specifies the range of source identifiers that this client will use. Since we have set the range to [0, 4) here, this client will be able to send up to four requests in flight at a time. 
+ `requestFifo`: a `boolean` option which defaults to false. If it is set to true, the client will request that downstream managers that support it send responses in the same order the corresponding requests were sent.
+ `visibility`: specifies the address ranges that the client will access. By default it is set to include all addresses. Use `AddressSet` to wrap the address it you want to custom it.

And you can connect your hardware logic to `tl` bundle in order to actually send/receive TileLink messages. The `edge` object represents the edge of the Diplomacy graph. It contains some useful helper functions.


####  `TLManagerNode`

The manager node is the slaver in TileLink spec.

> TileLink managers take requests from clients on the A channel and send responses back on the D channel.

##### Create a `TLManagerNode` 

> You can create a manager node using the `TLHelper` like so:
>
> ```scala
> class MyManager(implicit p: Parameters) extends LazyModule {
>   val device = new SimpleDevice("my-device", Seq("tutorial,my-device0"))
>   val beatBytes = 8
>   val node = TLHelper.makeManagerNode(beatBytes, TLManagerParameters(
>     address = Seq(AddressSet(0x20000, 0xfff)),
>     resources = device.reg,
>     regionType = RegionType.UNCACHED,
>     executable = true,
>     supportsArithmetic = TransferSizes(1, beatBytes),
>     supportsLogical = TransferSizes(1, beatBytes),
>     supportsGet = TransferSizes(1, beatBytes),
>     supportsPutFull = TransferSizes(1, beatBytes),
>     supportsPutPartial = TransferSizes(1, beatBytes),
>     supportsHint = TransferSizes(1, beatBytes),
>     fifoId = Some(0)))
> 
>   lazy val module = new LazyModuleImp(this) {
>     val (tl, edge) = node.in(0)
>   }
> }
> ```

##### Arguments Needed for `TLManagerode`

It need `beatBytes` and `TLManagerParameters`.

+ `beatBytes`: the physical width of the TileLink interface in bytes.

`TLManagerParameters` includes:

+ `address`: only required argument which is the set of address ranges that this manager will serve. This information is used to route requests from the clients. `AddressSet` is a mask, not a size. You should generally set it to be one less than a power of two (`1 << size -1`).

+ `resources`: usually retrieved from a `Device` object.

+ `regionType`: gives some information about the caching behavior of the manager.

  > There are seven region types, listed below:
  >
  > 1. `CACHED`      - An intermediate agent may have cached a copy of the region for you.
  > 2. `TRACKED`     - The region may have been cached by another master, but coherence is being provided.
  > 3. `UNCACHED`    - The region has not been cached yet, but should be cached when possible.
  > 4. `IDEMPOTENT`  - Gets return most recently put content, but content should not be cached.
  > 5. `VOLATILE`    - Content may change without a put, but puts and gets have no side effects.
  > 6. `PUT_EFFECTS` - Puts produce side effects and so must not be combined/delayed.
  > 7. `GET_EFFECTS` - Gets produce side effects and so must not be issued speculatively.

+ `executable`: determines if the CPU is allowed to fetch instructions from this manager. By default it is false, which is what most `MMIO` peripherals should set it to.

+ `support`:determine the different A channel message types that the manager can accept.

+ `fifoId` setting, which determines which FIFO domain (if any) the manager is in.

  > If this argument is set to `None` (the default), the manager will not guarantee any ordering of the responses. If the `fifoId` is set, it will share a FIFO domain with all other managers that specify the same `fifoId`. This means that client requests sent to that FIFO domain will see responses in the same order.

####  `TLResiterNode`

While you can directly specify a manager node and write all of the logic to handle TileLink requests, it is usually much easier to use a register node.

####  `TLIdentityNode`

Unlike the previous node types, which had only inputs or only outputs, the identity node has both.

This node is mainly used to combine multiple nodes into a single node with multiple edges and connect the client nodes with manager nodes.

In the following example, `MyClientGroup` will connect with `MyManagerGroup` with `manager.node :=* client.node`, and each of the node group contains two nodes.

> ```scala
> class MyClient1(implicit p: Parameters) extends LazyModule {
>   val node = TLHelper.makeClientNode("my-client1", IdRange(0, 1))
> 
>   lazy val module = new LazyModuleImp(this) {
>     // ...
>   }
> }
> 
> class MyClient2(implicit p: Parameters) extends LazyModule {
>   val node = TLHelper.makeClientNode("my-client2", IdRange(0, 1))
> 
>   lazy val module = new LazyModuleImp(this) {
>     // ...
>   }
> }
> 
> class MyClientGroup(implicit p: Parameters) extends LazyModule {
>   val client1 = LazyModule(new MyClient1)
>   val client2 = LazyModule(new MyClient2)
>   val node = TLIdentityNode()
> 
>   node := client1.node
>   node := client2.node
> 
>   lazy val module = new LazyModuleImp(this) {
>     // Nothing to do here
>   }
> }
> 
> class MyManager1(beatBytes: Int)(implicit p: Parameters) extends LazyModule {
>   val node = TLHelper.makeManagerNode(beatBytes, TLManagerParameters(
>     address = Seq(AddressSet(0x0, 0xfff))))
> 
>   lazy val module = new LazyModuleImp(this) {
>     // ...
>   }
> }
> 
> class MyManager2(beatBytes: Int)(implicit p: Parameters) extends LazyModule {
>   val node = TLHelper.makeManagerNode(beatBytes, TLManagerParameters(
>     address = Seq(AddressSet(0x1000, 0xfff))))
> 
>   lazy val module = new LazyModuleImp(this) {
>     // ...
>   }
> }
> 
> class MyManagerGroup(beatBytes: Int)(implicit p: Parameters) extends LazyModule {
>   val man1 = LazyModule(new MyManager1(beatBytes))
>   val man2 = LazyModule(new MyManager2(beatBytes))
>   val node = TLIdentityNode()
> 
>   man1.node := node
>   man2.node := node
> 
>   lazy val module = new LazyModuleImp(this) {
>     // Nothing to do here
>   }
> }
> 
> class MyClientManagerComplex(implicit p: Parameters) extends LazyModule {
>   val client = LazyModule(new MyClientGroup)
>   val manager = LazyModule(new MyManagerGroup(8))
> 
>   manager.node :=* client.node
> 
>   lazy val module = new LazyModuleImp(this) {
>     // Nothing to do here
>   }
> }
> ```

####  `TLAdapterNode`

+ Like the identity node, the adapter node takes some number of inputs and produces the same number of outputs. 
+ However, unlike the identity node, the adapter node does not simply pass the connections through unchanged. 
+ It can change the logical and physical interfaces between input and output and rewrite messages going through.

It includes `TLBuffer`, `TLFtagmenter`, `TLSourceShrinker`, `TLWidthWidget`, `TLFIFOFixer`, `TLROM`, `TLRAM ` and other similar nodes related to `AXI`.

####  `TLNexusNode`

> The nexus node is similar to the adapter node in that it has a different output interface than input interface. But it can also have a different number of inputs than it does outputs. This node type is mainly used by the `TLXbar` widget, which provides a TileLink crossbar generator.
> This has similar arguments as the adapter node’s constructor, but instead of taking single parameters objects as arguments and returning single objects as results, the functions take and return sequences of parameters.

## TileLink Edge Object Methods

### `Get`

Require data from memory via `A` channel and receive response via `D` channel.

**Arguments:**

- `fromSource: UInt` - Source ID for this transaction
- `toAddress: UInt` - The address to read from
- `lgSize: UInt` - Base two logarithm of the number of bytes to be read

**Returns:**

A `(Bool, TLBundleA)` tuple. The first item in the pair is a `boolean` indicating whether or not the operation is legal for this edge. The second is the A channel bundle.

### `Put`

Write data to memory via `A` channel.

> It will be a `PutPartial` if the `mask` is specified and a `PutFull` if it is omitted. The put may require multiple beats. If that is the case, only `data` and `mask` should change for each beat. All other fields must be the same for all beats in the transaction, including the address.

**Arguments:**

- `fromSource: UInt` - Source ID for this transaction.
- `toAddress: UInt` - The address to write to.
- `lgSize: UInt` - Base two logarithm of the number of bytes to be written.
- `data: UInt` - The data to write on this beat.
- `mask: UInt` - (optional) The write mask for this beat.

**Returns:**

A `(Bool, TLBundleA)` tuple. The first item in the pair is a `boolean` indicating whether or not the operation is legal for this edge. The second is the A channel bundle.

### Other Request Methods

+ `Arithmetic`
+ `Logical`
+ `Hint`
+ `AccessAck`
+ `HintAck`

### `first`

 True if the current beat is the first, or false otherwise.

### `last`

True if the current beat is the last, or false otherwise.

### `done`

True if the current beat is the last and a beat is sent on this cycle. False otherwise.

`x.fire() && last(x)`

### `count`

This method take a decoupled channel (either the A channel or D channel) and determines the count (starting from 0) of the current beat in the transaction.

### `numBeats`

This method takes in a TileLink bundle and gives the number of beats expected for the transaction.

### `numBeats1`

Similar to `numBeats` except it gives the number of beats minus one. If this is what you need, you should use this instead of doing `numBeats - 1.U`, as this is more efficient.

### `hasData`

Determines whether the TileLink message contains data or not. This is true if the message is a `PutFull`, `PutPartial`, `Arithmetic`, `Logical`, or `AccessAckData`.