---
layout: post
title: "[Tutorial] TileLink RegMap"
description: "The study of TileLink TLRegMap"
categories: [Tutorial]
tags: [Chisel, TileLink, RegMap]
last_updated: 2020-03-23 12:30:00 GMT+8
excerpt: "The study of TileLink TLRegMap"
redirect_from:
  - /2020/03/20/
---

* Kramdown table of contents
{:toc .toc}
# `RegMap`

## [Introduction](https://chipyard.readthedocs.io/en/latest/TileLink-Diplomacy-Reference/Register-Router.html)

>Memory-mapped devices generally follow a common pattern. They expose a set of registers to the CPUs. 
> + By writing to a register, the CPU can change the device’s settings or send a command. 
> + By reading from a register, the CPU can query the device’s state or retrieve results.

You can use `regmap` interface by extending the `TLRegisterRouter` class or you can create a regular `LaztModule` and instantiate a `TLRegisterNode`.

### Extends `TLRegisterRouter` 

If you want to use `RegMap` in TileLink, you need one `LazyModule` and one `LazyModuleImp`.

As for `LazyModule`, you can new one `TLRegisterRouter` with your own `trait`.

As for `LazyModuleImp`, you new one `TLRegModule` with your own `trait`.

### Instantiate a `TLRegisterNode`

The constructor has two required arguments: 

+ `address`: the **base address** of the registers

+ `device`: the device tree entry

There are also two optional arguments:

+ `beatBytes`: the interface width in bytes. The default value is 4 bytes. 
+ `concurrency` : the size of the internal queue for TileLink requests. By default, this value is 0, which means there will be no queue. This value must be greater than 0 if you wish to decoupled requests and responses for register accesses.

## `HasRegMap`

`regmapper/Regiled.scala:245`.

You can use function `regmap`, which is implemented in object `RegFiled` at `regmapper/Regiled.scala:167`. 

The registers applied with `regmap` have four types: write, read, write one to clear, R/W register.

```scala
  def r(n: Int, r: RegReadFn) 
  def r(n: Int, r: RegReadFn,  desc: RegFieldDesc) 
  def w(n: Int, w: RegWriteFn)
  def w(n: Int, w: RegWriteFn, desc: RegFieldDesc)
  // This RegField allows 'set' to set bits in 'reg'.
  // and to clear bits when the bus writes bits of value 1.
  // Setting takes priority over clearing.
  def w1ToClear(n: Int, reg: UInt, set: UInt, desc: Option[RegFieldDesc] = None)
  // This RegField wraps an explicit register
  // (e.g. Black-Boxed Register) to create a R/W register.
  def rwReg(n: Int, bb: SimpleRegIO, desc: Option[RegFieldDesc] = None)
```

So if you want to use `r` or `w`, you need to provide:

+ `n`: the size of the register
+ `r` or `w`: the register parameter
+ `desc`: one case class `RegFieldDesc`. You can simply use one, and provide at least `name` and `desc` in this case class. You can find it at `regmapper/Regiled.scala:38`.

One example can be found [here](https://www.chiselchina.com/forum/topic/10/%E5%9F%BA%E4%BA%8Etilelink%E7%9A%84dma%E8%AE%BE%E8%AE%A11-%E5%AD%98%E5%82%A8%E6%98%A0%E5%B0%84%E5%AF%84%E5%AD%98%E5%99%A8%E6%8E%A7%E5%88%B6dma%E8%BF%90%E8%A1%8C):

```scala
val reqReg = RegInit(0.U(64.W))
val compReg = RegInit(false.B)
regmap(
  0x00 -> Seq(RegField.w(64, reqReg)), // the address of reqReg is 0x00
  0x08 -> Seq(RegField.r(1,compReg)) // the address of compReg is 0x08
)
```

The first element of the pair is **an offset from the base address**. The second is a sequence of `RegField` objects, each of which maps a different register. The `RegField` constructor takes two arguments. The first argument is the width of the register in bits. The second is the register itself.

## `RegisterRouter`

Extend from `LazyModule`.

## `TLRegisterRouter`

`tilelink/RegisterRouter.scala:152`.

Extends from `TLRegsiterRouterBase` at `tilelink/RegisterRouter.scala:119`, which further extends from `LazyModule`. It need the following parameters:

```scala
(
     val base:        BigInt,
     val devname:     String,
     val devcompat:   Seq[String],
     val interrupts:  Int     = 0,
     val size:        BigInt  = 4096,
     val concurrency: Int     = 0,
     val beatBytes:   Int     = 4,
     val undefZero:   Boolean = true,
     val executable:  Boolean = false)
   (bundleBuilder: TLRegBundleArg => B)
   (moduleBuilder: (=> B, TLRegisterRouterBase) => M)(implicit p: Parameters)
```

And `TLRegisterRouterBase` needs to be `LazyModuleImp`.

## `TLRegisterNode`

Since the argument is a sequence, you can associate multiple `RegField` objects with an offset. If you do, the registers are read or written in parallel when the offset is accessed. The registers are in little endian order, so the first register in the list corresponds to the least significant bits in the value written. In this example, if the CPU wrote to offset 0x0E with the value 0xAB, `smallReg0` will get the value 0xB and `smallReg1` would get 0xA.

```scala
import chisel3._
import chisel3.util._
import freechips.rocketchip.config.Parameters
import freechips.rocketchip.diplomacy._
import freechips.rocketchip.regmapper._
import freechips.rocketchip.tilelink.TLRegisterNode

class MyDeviceController(implicit p: Parameters) extends LazyModule {
  val device = new SimpleDevice("my-device", Seq("tutorial,my-device0"))
  val node = TLRegisterNode(
    address = Seq(AddressSet(0x10028000, 0xfff)),
    device = device,
    beatBytes = 8,
    concurrency = 1)

  lazy val module = new LazyModuleImp(this) {
    val bigReg = RegInit(0.U(64.W))
    val mediumReg = RegInit(0.U(32.W))
    val smallReg = RegInit(0.U(16.W))

    val tinyReg0 = RegInit(0.U(4.W))
    val tinyReg1 = RegInit(0.U(4.W))

    node.regmap(
      0x00 -> Seq(RegField(64, bigReg)),
      0x08 -> Seq(RegField(32, mediumReg)),
      0x0C -> Seq(RegField(16, smallReg)),
      0x0E -> Seq(
        RegField(4, tinyReg0),
        RegField(4, tinyReg1)))
  }
}
```

