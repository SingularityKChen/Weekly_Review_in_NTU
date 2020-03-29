---
layout: post
title: "[CodeReview] RocketChip Fuzzer"
description: "The study of SiFive TileLink"
categories: [CodeReview]
tags: [Chisel, TileLink]
last_updated: 2020-03-29 14:48:00 GMT+8
excerpt: "Code review of fuzzer in rocketchip."
redirect_from:
  - /2020/03/29/
---

* Kramdown table of contents
{:toc .toc}
# RocketChip Fuzzer

Location: `./rocketchip/src/main/scala/tilelink/Fuzzer.scala`.

We should learn two class in this file and know how to get/put/Hint, etc. via TileLink protocol:

+ `IDMapGenerator`
+ `TLFuzzer`

The original one was written in Chisel2 coding style and [Sequencer ](https://github.com/sequencer) modified it into Chisel3 coding style [here](https://github.com/sequencer/rocket-chip/blob/2e2af304c049e26256ca545d470e05bb26e701ce/src/main/scala/tilelink/Fuzzer.scala). And I will introduce the Chisel3 one.

## IDMapGenerator

This module can be seen as a demo to manage the source id, including how to send source id for requirement and how to reset the source id while it receive the response.

<script src="http://gist-it.appspot.com/https://github.com/sequencer/rocket-chip/blob/2e2af304c049e26256ca545d470e05bb26e701ce/src/main/scala/tilelink/Fuzzer.scala?slice=10:37" ></script>

`io.free` is used to receive the response source id via `TileLink` channel D, `io.alloc` is used to send the require source id via `TileLink` channel A.

And the `bitmap` register has `numIds` bits, **each bit represents one source id**, hence both require source id and response source id will be transmitted between one hot and common binary. The `true` means idle and `false` means this bit's source has sent requirement and hasn't received response.

While `io.alloc.fire()` is true, then it will send a source id via `io.alloc.bits`. This value is transmitted to one hot and assigned to `clr`, which is used to assign this bit to `false` in `bitmap`, means it's unavailable now.

```scala
  val clr = WireDefault(0.U(numIds.W))
  when (io.alloc.fire()) { clr := UIntToOH(io.alloc.bits) }
```

While `io.free.fire()` is true, then it receives one response source id via `io.free.bits`. This value is transmitted to one hot too and assigned to `set`, which is used to assign this bit to `true` in `bitmap`, means it's available now.

```scala
  val set = WireDefault(0.U(numIds.W))
  when (io.free.fire()) { set := UIntToOH(io.free.bits) }
```

The `select` signal means the position of lowest bits which value is true. And it uses `leftOR` to let all the bits of the left side of the lowest `true` bit are `true`, then shift left one bit, then get its negative value, `and` it with original `bitmap`, hence get the one hot value.

```scala
  val select = (~(leftOR(bitmap) << 1)).asUInt & bitmap
  io.alloc.bits := OHToUInt(select)
  io.alloc.valid := bitmap.orR()
```

## TLFuzzer

This module is the main body to communicate with other TileLink components.

This module will not finish until it sends/receives `nOperations`.

### Create a Master Node

Firstly, it create a `clientParams`.

<script src="http://gist-it.appspot.com/https://github.com/sequencer/rocket-chip/blob/2e2af304c049e26256ca545d470e05bb26e701ce/src/main/scala/tilelink/Fuzzer.scala?slice=89:104" ></script>

Then use it to create a client node, i.e., a master node.

```scala
val node = TLClientNode(Seq(TLClientPortParameters(clientParams)))
```

For more details of client node parameter, you can see [here](https://singularitykchen.github.io/blog/2020/03/21/Tutorial-TileLink/) at [my blog](https://singularitykchen.github.io/blog/).

### Get Master and Salver

```scala
    val (out, edge) = node.out(0)
```

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200329134935TileLinkFiveChannels.png" alt="TileLink Five Channels" style="zoom: 50%;" />

Here, as we create a client node, or master node. So `out` represents the *Master Interface* in the graph, `edge` represents the *Slaver Interface* in the graph.

### Generate TileLink Messages

From line 156 to line 171, it shows how to generate the TileLink messages such as put, get, Hint, etc. And only the `edge` owns those methods.

<script src="http://gist-it.appspot.com/https://github.com/sequencer/rocket-chip/blob/2e2af304c049e26256ca545d470e05bb26e701ce/src/main/scala/tilelink/Fuzzer.scala?slice=154:171" ></script>

### Get Legal Message

`legal_dest` is used to see whether current address is legal.

```scala
    val legal_dest = edge.manager.containsSafe(addr)
```

And each TileLink operation will produce one kind of legal message, so it use mux to get the current operation's legal message.

```scala
    val legal = legal_dest && MuxLookup(a_type_sel, glegal, Seq(
      "b000".U -> glegal,
      "b001".U -> (pflegal && !noModify.B),
      "b010".U -> (pplegal && !noModify.B),
      "b011".U -> (alegal && !noModify.B),
      "b100".U -> (llegal && !noModify.B),
      "b101".U -> hlegal))
```

### Assign ReadVaild Signals

It need to know the process of each operation, so 

```scala
    // Progress within each operation
    val a = out.a.bits
    val (a_first, a_last, req_done) = edge.firstlast(out.a)

    val d = out.d.bits
    val (d_first, d_last, resp_done) = edge.firstlast(out.d)
```

```scala
    // Wire up Fuzzer flow control
    val a_gen = if (nOperations>0) num_reqs =/= 0.U else true.B
    idMap.io.alloc.ready := a_gen && legal && a_first && out.a.ready
    idMap.io.free.valid := d_first && out.d.fire()
    idMap.io.free.bits := out.d.bits.source
```

Only when `a_first` is true, then `idMap` need to generate one source id.

When `d_fisrt` is true, then the response is coming, and `idMap` can set this response source.

### Tie Off the Unused Channels

As this module uses channel a, b and d, doesn't use channel c and e, so

```scala
    out.a.valid := !reset.asBool && a_gen && legal && (!a_first || idMap.io.alloc.valid)
    out.b.ready := true.B
    out.c.valid := false.B
    out.d.ready := true.B
    out.e.valid := false.B
```

## Another Mimic Demo

I [changed the idMap generator](https://github.com/SingularityKChen/dl_accelerator/blob/fdd71c01cb85aa6556958fd421a6888450fd6e98/dla/src/diplomatic/MemCtrl.scala#L89) where each source id only sends one requirement by using two different registers `reqBitmap` and `respBitmap`.

<script src="http://gist-it.appspot.com/https://github.com/SingularityKChen/dl_accelerator/blob/fdd71c01cb85aa6556958fd421a6888450fd6e98/dla/src/diplomatic/MemCtrl.scala?slice=88:126" ></script>