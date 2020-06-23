---
layout: post
title: "[Tutorial] Suggest Using private Before val"
description: "Why we need to use private val in Chisel"
categories: [Tutorial]
tags: [Chisel]
last_updated: 2020-04-02 14:48:00 GMT+8
excerpt: "This tutorial suggests to use private as a prefix of val when create a wire or register and mentions one possible problem when using private."
redirect_from:
  - /2020/04/03/
---

* Kramdown table of contents
{:toc .toc}
# Suggest Using `private` Before `val`

## Why we need to use `private` before `val`

In a real hardware, we can only access the data via interface, and other registers, wires can be seen only inside this module. So I think we'd better to use `private` before `val` every time we create a new register or wire, hence we can not access those outside this module.

``` scala
val io: ClusterGroupIO = IO(new ClusterGroupIO)
private val peCluster = Module(new PECluster(debug)).io
private val glbInActWriteFinReg = Seq.fill(inActSRAMNum){RegInit(false.B)}
```

Though if you try to access a register from other module, the compiler will tell you that you may need to add `IO()` or `Module()`, I prefer this safer way.

## How to keep the variable's name when generating `Firrtl`?

However, if you use `private` as a prefix directedly, then when you try to generate Chisel into Firrtl, you will find your circuit is encrypted, and it only keep the name of interfaces(IOs).

The method bellow maybe helpful.

You need to add a case with prefix `private` inside [function `transform`](https://github.com/freechipsproject/chisel3/blob/fbf5e6f1a0e8bf535d465b748ad554575fe62156/macros/src/main/scala/chisel3/internal/naming/NamingAnnotations.scala#L63):

```scala
override def transform(tree: Tree): Tree = tree match {
    // Intentionally not prefixed with $mods, since modifiers usually mean the val definition
    // is in a non-transformable location, like as a parameter list.
    // TODO: is this exhaustive / correct in all cases?
    case q"val $tname: $tpt = $expr" => {
        val TermName(tnameStr: String) = tname
        val transformedExpr = super.transform(expr)
        q"val $tname: $tpt = $contextVar.name($transformedExpr, $tnameStr)"
    }
    case q"private val $tname: $tpt = $expr" => {
        val TermName(tnameStr: String) = tname
        val transformedExpr = super.transform(expr)
        q"val $tname: $tpt = $contextVar.name($transformedExpr, $tnameStr)"
    }
    case other => super.transform(other)
}
```

Then you can keep each variable's name into Firrtl as usual.