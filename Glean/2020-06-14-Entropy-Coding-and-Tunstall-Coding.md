---
layout: post
title: "[Glean] Entropy Coding and Tunstall Coding"
description: "This post introduces the concepts of entropy coding and Tunstall coding."
categories: [Glean]
tags: [Tunstall, Entropy]
last_updated: 2020-06-14 21:12:00 GMT+8
excerpt: "This post introduces the concepts of entropy coding and Tunstall coding."
redirect_from:
  - /2020/06/14/
---

* Kramdown table of contents
{:toc .toc}
# Entropy Coding and Tunstall Coding

## Entropy Coding

An entropy encoding is **a lossless data compression scheme** that is independent of the specific characteristics of the medium.

Two of the most common entropy encoding techniques are **Huffman coding** and **arithmetic coding**.

## Tunstall Coding[^1]

+ Select the symbol with largest probability in the table. Call it `S`. 
+ Remove `S` and add the `N` substrings `Sx` where `x` goes over all the `N` symbols. This step increase the table size by `N−1`symbols (some of them may be substrings). Thus, after iteration `k`, the table size will be `N + k(N − 1)` elements.
+ If `N + k(N − 1) ≤ 2n`, perform another iteration.

<img src="https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20200614202435.png" alt="Tunstall Coding" style="zoom:67%;" />

An important property of the Tunstall codes is their reliability. If one bit becomes corrupt, only one code will get bad. Normally, variable-size codes do not feature any reliability.

[^1]: Salomon D. Data compression: the complete reference[M]. Springer Science & Business Media, 2004.