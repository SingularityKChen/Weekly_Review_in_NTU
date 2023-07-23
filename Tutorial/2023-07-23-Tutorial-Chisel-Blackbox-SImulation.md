---
layout: post
title: "[Tutorial] Chisel simulation with the hierarchical BlackBox Module"
description: "This article introduces how to do Chisel simulation with the hierarchical BlackBox Module."
categories: [Tutorial]
tags: [Chisel, BlackBox]
last_updated: 2023-07-23 19:11:00 GMT+8
excerpt: "This article introduces how to do Chisel simulation with the hierarchical BlackBox Module."
redirect_from:
  - /2023/07/23/
---

* Kramdown table of contents
{:toc .toc}

# Chisel Backbox Parameterization

For basic introduction, you can refer to Chisel Official Document: [BlackBoxes](https://www.chisel-lang.org/chisel3/docs/explanations/blackboxes.html#parameterization).

Parameters can be passed into the Verilog module by passing a `Map()` to the `params` argument of the `BlackBox` constructor. The keys of the map are the parameter names, and the values are the parameter values.

For example, in [the following code](https://github.com/SingularityKChen/chisel-chipware/blob/8695097f658798ac6db0f3ba3a3b879bb5648955/chipware/src/CW_add.scala#L53-L62), the `CW_add` module has a parameter `wA` which is passed into the Verilog module by `Map("wA" -> wA)`.

```scala
class CW_add(val wA: Int = 4) extends BlackBox(Map("wA" -> wA)) with HasBlackBoxPath {
  require(wA >= 1, "wA must be >= 1")
  val io = IO(new Bundle {
    val A:  UInt = Input(UInt(wA.W))
    val B:  UInt = Input(UInt(wA.W))
    val CI: UInt = Input(UInt(1.W))
    val Z:  UInt = Output(UInt(wA.W))
    val CO: UInt = Output(UInt(1.W))
  })
}
```

# Chisel Simulation with Blackbox

If we want to do simulation with Blackbox modules, we need to provide the Verilog file of the Blackbox module.

This can be done by using the `addResource` method of the `HasBlackBoxResource` trait.

Besides, the trait `HasBlackBoxPath` provides a `addPath` method that can be used to specify the Verilog file(s) of the Blackbox module.
This method is suggested when the Verilog file is larger than 1MB.

# Chisel Simulation with Hierarchical Blackbox

However, sometimes, the Blackbox module is not a single Verilog file, but a hierarchical Verilog module.
In this case, we need to provide the Verilog files of the hierarchical Blackbox module.
This can be done by calling the `addPath` method multiple times.

Below is [an example](https://github.com/SingularityKChen/chisel-chipware/blob/8695097f658798ac6db0f3ba3a3b879bb5648955/chipware/test/src/DemoTest.scala#L1-L57) of using hierarchical Blackbox module in Chisel simulation.

Line 24 calls the `addPath` method of the `CW_add` module to specify the Verilog file of the hierarchical Blackbox module multiple times.

```scala
import chisel3._
import chiseltest._
import chiseltest.simulator.VerilatorFlags
import firrtl2.annotations.NoTargetAnnotation
import utest._

class addWithBlackBox(val wA: Int = 4) extends Module {
  require(wA >= 1, "wA must be >= 1")
  val io = IO(new Bundle {
    val A:  UInt = Input(UInt(wA.W))
    val B:  UInt = Input(UInt(wA.W))
    val CI: UInt = Input(UInt(1.W))
    val Z:  UInt = Output(UInt(wA.W))
    val CO: UInt = Output(UInt(1.W))
  })
  protected val U1: CW_add = Module(new CW_add(wA))
  U1.io.A  := io.A
  U1.io.B  := io.B
  U1.io.CI := io.CI
  io.Z     := U1.io.Z
  io.CO    := U1.io.CO

  def addPath(blackBoxFileSeq: Seq[String]): Unit = {
    blackBoxFileSeq.foreach(blackBoxPath => U1.addPath(blackBoxPath))
  }
}

object addWithBlackBox {
  def simWithBBoxVerilog(blackBoxFileSeq: Seq[String]): addWithBlackBox = {
    val addWithBlackBox = new addWithBlackBox
    addWithBlackBox.addPath(blackBoxFileSeq)
    addWithBlackBox
  }
}

object DemoTest extends TestSuite with ChiselUtestTester {
  val bboxFileSeq: Seq[String] = Seq(
    "chipware/test/resources/CW_add_demo.v"
  )
  val annotationSeq: Seq[NoTargetAnnotation] = Seq(
    WriteVcdAnnotation,
    VerilatorBackendAnnotation,
    // Disable all lint warnings
    VerilatorFlags(Seq("-Wno-lint"))
  )
  val tests: Tests = Tests {
    test("add should work with blackbox") {
      testCircuit(addWithBlackBox.simWithBBoxVerilog(bboxFileSeq), annotationSeq = annotationSeq) { dut =>
        dut.io.A.poke(1.U)
        dut.io.B.poke(2.U)
        dut.io.CI.poke(0.U)
        dut.io.Z.expect(3.U)
        dut.io.CO.expect(0.U)
      }
    }
  }
}
```