---
layout: post
title: "[Glean] Einsum Introduction"
description: "This post introduces Einsum and its applications."
categories: [Glean]
tags: [Einsum, Einstein Summation Convention, Python, NumPy]
last_updated: 2024-08-19 17:06:00 GMT+8
excerpt: "This post introduces Einsum and its applications."
redirect_from:
  - /2024/03/06/
---

* Kramdown table of contents
{:toc .toc}

# Einsum[^1][^2]

Einsum is short for Einstein summation convention, which is a compact representation for combining products and sums in a general way. It is proposed by Einstein in 1916 to simplify tensor operations. In short, it is a shorthand notation for summation.
It is a concise way to specify operations on multidimensional arrays. The syntax of einsum is as follows:

```python
numpy.einsum(subscripts, *operands, out=None, dtype=None, order='K', casting='safe', optimize=False)
```

The `subscripts` argument is a string that specifies the subscripts for summation. The `operands` argument is a list of arrays to be operated on. The `out` argument is an optional output array. The `dtype` argument is an optional data type. The `order` argument is an optional memory layout. The `casting` argument is an optional casting rule. The `optimize` argument is an optional optimization flag.

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/202408251507471-Einsum_py.png"/>

Einsum could be used to replace but not limited to the following functions:
- Matrix trace: trace
- Matrix diagonal: diag
- Tensor summation (along axes): sum
- Tensor transpose: transpose
- Matrix multiplication: dot
- Tensor multiplication: tensordot
- Vector inner product: inner
- Outer product: outer

## `subscripts` argument

The `subscripts` argument is a string that specifies the subscripts for summation. The subscripts consist of comma-separated subscript labels, where each label refers to a dimension of the corresponding operand. The labels are used to specify the dimensions of the operands to be multiplied and summed. The Einstein summation convention implies that a label that appears twice in the subscripts is to be summed over.

The detailed rules for the `subscripts` argument with examples are as follows:

- A label that appears only once in the subscripts is not summed over.
    - `i` means the `i-th` dimension of the first operand.
- A label that appears twice in the subscripts is summed over.
    - `ij` means the sum of the product of the `i-th` dimension of the first operand and the `j-th` dimension of the second operand.
- Comma-separated labels in the subscripts imply a direct product.
    - `i,j` means the direct product of the `i-th` dimension of the first operand and the `j-th` dimension of the second operand.
- An ellipsis (`...`) allows for an arbitrary number of dimensions.
    - `i...` means all dimensions of the first operand except the first one.
- `->` separates the input and output operands.
    - `ij,jk->ik` means the sum of the product of the `i-th` dimension of the first operand and the `j-th` dimension of the second operand, and the result is stored in the `k-th` dimension of the output operand.

## Example

```python
import numpy as np

a = np.array([[1, 2], [3, 4]])
b = np.array([[5, 6], [7, 8]])

# Matrix multiplication
c = np.einsum('ij,jk->ik', a, b)
# c = np.dot(a, b)

# Matrix trace
d = np.einsum('ii', a)
# d = np.trace(a)

# Tensor summation
e = np.einsum('ijk->k', np.arange(24).reshape(2, 3, 4))
# e = np.sum(np.arange(24).reshape(2, 3, 4), axis=(0, 1))

# Tensor transpose
f = np.einsum('ijk->kji', np.arange(24).reshape(2, 3, 4))
# f = np.transpose(np.arange(24).reshape(2, 3, 4), axes=(2, 1, 0))

# Vector inner product
g = np.einsum('i,i', np.arange(3), np.arange(3))
# g = np.inner(np.arange(3), np.arange(3))

# Outer product
h = np.einsum('i,j->ij', np.arange(3), np.arange(3))
# h = np.outer(np.arange(3), np.arange(3))
```

# Reference

[^1]: [Einsum is All you Need - Einstein Summation in Deep Learning](https://rockt.github.io/2018/04/30/einsum)
[^2]: [一个函数打天下，einsum](https://zhuanlan.zhihu.com/p/71639781)

