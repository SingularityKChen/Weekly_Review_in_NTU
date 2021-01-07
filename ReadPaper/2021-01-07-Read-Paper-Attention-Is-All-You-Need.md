---
layout: post
title: "[Read Paper] Attention Is All You Need"
description: "This blog is the combination of two blogs which introduces the paper Attention is All You Need. Shortages and one improvement is shown, too."
categories: [ReadPaper]
tags: [Transformer, NLP, Attention]
last_updated: 2021-01-07 15:57:00 GMT+8
excerpt: "This blog is the combination of two blogs which introduces the paper Attention is All You Need. Shortages and one improvement is shown, too."
redirect_from:
  - /2021/01/07/
---

* Kramdown table of contents
{:toc .toc}
# Attention Is All You Need[^1][^2]

## Transformer: Encoder-Decoder Structure

![Transformer Structure](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20210107110545.png)The encoding component is **a stack of encoders**. The decoding component is a stack of decoders of the same number.

## Structure of Encoder and Decoder

![Structure of Encoder and Decoder](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20210107113045.png)

+ **Self-attention layer**: a layer that helps the encoder look at other words in the input sentence as it encodes a specific word
+ **Encoder-decoder attention** **layer**: helps the decoder focus on relevant parts of the input sentence

## Sequential Coding: From Sentence to Vector

Every sentence is a matrix $$X = (x_1, x_2, ..., x_t)$$ where $$x_i$$ is the $i^{th}$ word vector.

We have three methods to encode these sequences.

### RNN: LSTM, GRU, SRU

Doing this recursively: $$y_t=f(y_{t-1}, x_t)$$

Shortage:

+ cannot computing parallelly
+ can not study a global structure information as it's a **Markov Decision Process**.

### CNN: Seq2Seq

Convolution: $$y_t = f(x_{t-1}, x_t, x_{t+1})$$

CNN is convenient to compute parallelly and can capture some local information, which can be increased with deeper layers.

### Attention

$$y_t=f(x_t, A, B)$$

Where $A$ and $B$ are other matrix. If $$A=B=X$$, then it is called **Self Attention**, which means $y_t$ is computed by comparing $x_t$ with every words in the sentence.

## Encoding Component

An encoder receives a list of vectors as input. It processes this list by passing these vectors into a ‘self-attention’ layer, then into a feed-forward neural network, then sends out the output upwards to the next encoder.

![](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20210107114315.png)

### Computation of Attention

![Scaled Dot-Product Attention](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20210107143430.png)

The **first step** in calculating self-attention is to create three vectors from each of the encoder’s input vectors.

![Three Vectors of the Self-Attention](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20210107143113.png)

There are seven steps to compute the attention:

1. translate words into embedding vectors
2. compute three vectors $Q$, $K$, $V$, where $Q=X \times W^Q$, $K=X \times W^K$, $V=X \times W^V$.
3. compute the dot product of $Q$ with $V$ of the respective word.
4. normalize the scores by dividing the scores with $\sqrt{d_k}$.
5. softmax normalizes the scores.
6. multiply each value vector by the softmax score. The intuition here is to keep intact the values of the word(s) we want to focus on, and drown-out irrelevant words.
7. sum up the weighted value vectors.

![Six steps of Computing Attention](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20210107143812.png)

In fact, we can condense steps two through seven in one formula to calculate the outputs of the self-attention layer.

![The self-attention calculation in matrix form](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20210107150504.png)

Actually, it is three matrix multiplication: $n \times d_k$, $d_k \times m$, $m \times d_v$, finally get a matrix $n \times d_v$. Therefore, attention layer is a process that encode $n \times d_k$ sequence $Q$  into a new $n \times d_v$ sequence.

### Multi-Head Attention

The paper further refined the self-attention layer by adding a mechanism called “multi-headed” attention. This improves the performance of the attention layer in two ways:

1. It expands the model’s ability to focus on different positions.
2. It gives the attention layer multiple “representation subspaces”.

$$head_i=Attention(QW_i^q, KW_i^q, VW_i^q)$$

$$MultiHead(Q, K, V)=Concat(head_1, ..., head_h)$$

## Shortages of Attention Layers

Though attention is able to capture the global information by comparing the sequences while the computation complex is $$O(n^2)$$, however, it still has some shortages:

1. both multi-head attention and residual structure are from CNN
2. can not model the location information.
3. in some situation, applications relies on partial information.

Therefore, maybe we can combine attention with CNN or RNN and fully utilize their own advantages.

## One Improvement: RealFormer: Transformer Likes Residual Attention

Attention:

$$Attention(Q_n, K_n, V_n)=softmax(A_n)V_n, A_n=\frac{Q_nK_n^T}{\sqrt{d_k}}$$

New attention:

$$Attention(Q_n, K_n, V_n)=softmax(A_n)V_n, A_n=\frac{Q_nK_n^T}{\sqrt{d_k}}+A_{n-1}$$

![Comparison of different Transformer layers](https://raw.githubusercontent.com/SingularityKChen/PicUpload/master/img/20210107155111.png)

[^1]: The Illustrated Transformer, https://jalammar.github.io/illustrated-transformer/
[^2]: 《Attention is All You Need》浅读（简介+代码）, https://kexue.fm/archives/4765