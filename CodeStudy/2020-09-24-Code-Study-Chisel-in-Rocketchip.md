---
layout: post
title: "[CodeStudy] Some Chisel details in the project RocketChip"
description: "Learned some tips of Chisel via RocketChip. Here includes come implicit classes, and one implementation of a gray counter."
categories: [CodeStudy]
tags: [Chisel, RoeketChip, grey, implicit class]
last_updated: 2020-09-24 22:21:00 GMT+8
excerpt: "Learned some tips of Chisel via RocketChip. Here includes come implicit classes, and one implementation of a gray counter."
redirect_from:
  - /2020/09/24/
---

* Kramdown table of contents
{:toc .toc}
# implicit classes

`rocketchip/src/main/scala/util/package.scala`

## `holdUnless`

```scala
implicit class DataToAugmentedData[T <: Data](val x: T) extends AnyVal {
    def holdUnless(enable: Bool): T = Mux(enable, x, RegEnable(x, enable))
}
```

When `enable` is true, then current value is `x` and at the same time, the value in the register will change to `x` as well.

Hence, when `enable` is false, then the current value will be the value in the register `x`.

## `BooleanToAugmentedBoolean`

```scala
implicit class BooleanToAugmentedBoolean(val x: Boolean) extends AnyVal {
    def toInt: Int = if (x) 1 else 0

    // this one's snagged from scalaz
    def option[T](z: => T): Option[T] = if (x) Some(z) else None
}
```

`option` could used to new some conditional ports.

Example: `rocketchip/src/main/scala/util/AsyncQueue.scala`

```scala
class AsyncBundle[T <: Data](private val gen: T, val params: AsyncQueueParams = AsyncQueueParams()) extends Bundle {
  // Data-path synchronization
  ...
  val index = params.narrow.option(Input(UInt(params.bits.W)))

  // Signals used to self-stabilize a safe AsyncQueue
  val safe = params.safe.option(new AsyncBundleSafety)
}
```

# Gray Counter

`rocketchip/src/main/scala/util/AsyncQueue.scala`

```scala
object GrayCounter {
  def apply(bits: Int, increment: Bool = true.B, clear: Bool = false.B, name: String = "binary"): UInt = {
    val incremented = Wire(UInt(bits.W))
    val binary = RegNext(next=incremented, init=0.U).suggestName(name)
    incremented := Mux(clear, 0.U, binary + increment.asUInt())
    incremented ^ (incremented >> 1)
  }
}
```

Although it outputs gray codes, but need more resources than common codes here.

