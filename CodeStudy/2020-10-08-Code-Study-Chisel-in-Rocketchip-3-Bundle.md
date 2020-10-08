---
layout: post
title: "[CodeStudy] RocketChip Optional Bundle"
description: "Learned some tips of Chisel via RocketChip. This introduces how to make the bundles be optional."
categories: [CodeStudy]
tags: [Chisel, RocketChip]
last_updated: 2020-10-08 19:57:00 GMT+8
excerpt: "Learned some tips of Chisel via RocketChip. This introduces how to make the bundles be optional."
redirect_from:
  - /2020/10/08/
---

* Kramdown table of contents
{:toc .toc}
# Optional Bundle

`rocket-playground/rocketchip/src/main/scala/util/LanePositionedQueue.scala`

Sometimes, we need some ports but sometimes not. One method is `extend` one base class, or `with` one trait bundle. However, most time they're not that configurable.

Another way is making this bundle be `Optional`.

## Optional Basic

Optional means one parameter is either None or has some values.

For instance, in `Optional[T]`, if it contains some values, then its `Some[T]`, otherwise, that's `None`.

## Optional Functions

You can use `get()` to access `Some[T]` or `None`. Also, if you need one default value when `None`, then you can use `getOrElse()`.

## Making Bundle Optional

```scala
val free = if (args.free) Some(Flipped(Decoupled(UInt(depthBitsU.W)))) else None
val rewind = if (args.rewind) Some(Input(Bool())) else None
```

Simply wrapping your bundle with `Some()`.

When you want to get this value, you need to add `.get()` or `.getOrElse()`.

```scala
val f = q.io.free.get()
val doRewind = io.rewind.getOrElse(false.B)
```

In this case, bundles are more reusable and configurable.

