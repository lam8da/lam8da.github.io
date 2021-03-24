---
layout: post
title: 《线性代数应该这样学》笔记（下）
date: 2019-07-22 22:04:32.601622
categories:
    - 数学&物理
tags:
    - Work in progress
    - 代数
    - 线性代数
---

{::comment} Not show line_numbers {:/comment}
{::options syntax_highlighter_opts="{default_lang: c++ \}" /}

* TOC
{:toc}

# 内积空间

向量空间研究的主要是空间的线性结构（加法和标量乘法），而内积空间则加上了对应于二维空间和三维空间中长度和角度的概念。

## 内积

- 实向量空间或复向量空间$$V$$上的内积（inner product）就是一个函数，它把$$V$$中元素的每个有序对$$(\pmb{u},\pmb{v})$$都映成一个数$$\langle\pmb{u},\pmb{v}\rangle\in\mathbf{F}$$，并且具有性质：
  - 正性：对所有$$\pmb{v}\in V$$都有$$\langle\pmb{v},\pmb{v}\rangle\ge0$$
  - 定性：$$\langle\pmb{v},\pmb{v}\rangle=0$$当且仅当$$\pmb{v}=0$$
  - 第一个位置的加性：对所有$$\pmb{u},\pmb{v},\pmb{w}\in V$$都有$$\langle\pmb{u+v},\pmb{w}\rangle=\langle\pmb{u},\pmb{w}\rangle+\langle\pmb{v},\pmb{w}\rangle$$
  - 第一个位置的齐性：对所有$$a\in\mathbf{F},\pmb{v},\pmb{w}\in V$$都有$$\langle a\pmb{v},\pmb{w}\rangle=a\langle\pmb{v},\pmb{w}\rangle$$
  - 共轭对称性：对所有$$\pmb{v},\pmb{w}\in V$$都有$$\langle\pmb{v},\pmb{w}\rangle=\overline{\langle\pmb{w},\pmb{v}\rangle}$$

# Helpers

```
a T in L(V,W): T\in\mathcal{L}(V)
b       <a,b>: \langle\pmb{u},\pmb{v}\rangle
e      rangeT: \text{range}T
f       nullT: \text{null}T
p            : p(z)=a_{0}+a_{1}z+a_{2}z^{2}+\cdots+a_{m}z^{m}
l   lambda1-m: \lambda_{1},\cdots,\lambda_{m}
u       u1-um: (\pmb{u}_{1},\cdots,\pmb{u}_{m})
v       v1-vn: (\pmb{v}_{1},\cdots,\pmb{v}_{n})
w       w1-wm: (\pmb{w}_{1},\cdots,\pmb{w}_{m})

```

