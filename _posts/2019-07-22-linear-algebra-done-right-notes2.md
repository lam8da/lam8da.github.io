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
  - 正性：对所有$$\pmb{v}\in V$$都有$$\langle\pmb{v},\pmb{v}\rangle\ge0$$（这意味着$$\langle\pmb{v},\pmb{v}\rangle$$是一个实数）
  - 定性：$$\langle\pmb{v},\pmb{v}\rangle=0$$当且仅当$$\pmb{v}=0$$
  - 第一个位置的加性：对所有$$\pmb{u},\pmb{v},\pmb{w}\in V$$都有$$\langle\pmb{u}+\pmb{v},\pmb{w}\rangle=\langle\pmb{u},\pmb{w}\rangle+\langle\pmb{v},\pmb{w}\rangle$$
    - 通过这个和下一个性质可以证明第二个位置也具有加性，即：$$\langle\pmb{u},\pmb{v}+\pmb{w}\rangle=\langle\pmb{u},\pmb{v}\rangle+\langle\pmb{u},\pmb{w}\rangle$$
    - 可证$$\langle\pmb{0},\pmb{w}\rangle=\langle\pmb{w},\pmb{0}\rangle=0,\pmb{w}\in V$$
  - 第一个位置的齐性：对所有$$a\in\mathbf{F},\pmb{v},\pmb{w}\in V$$都有$$\langle a\pmb{v},\pmb{w}\rangle=a\langle\pmb{v},\pmb{w}\rangle$$
    - 通过这个和上一个性质可以证明第二个位置具有共轭齐性，即：$$\langle\pmb{u},a\pmb{v}\rangle=\overline{a}\langle\pmb{u},\pmb{v}\rangle$$
  - 共轭对称性：对所有$$\pmb{v},\pmb{w}\in V$$都有$$\langle\pmb{v},\pmb{w}\rangle=\overline{\langle\pmb{w},\pmb{v}\rangle}$$
    - 对实向量空间这等价于$$\langle\pmb{v},\pmb{w}\rangle=\langle\pmb{w},\pmb{v}\rangle$$
- 内积空间是带有内积的向量空间。例子：
  - $$\mathbf{F}^{n}$$，内积定义为$$\langle(w_{1},\cdots,w_{n}),(z_{1},\cdots,z_{n})\rangle=w_{1}\overline{z_{1}}+\cdots+w_{n}\overline{z_{n}}$$（这称为欧几里得内积）
  - 同样为$$\mathbf{F}^{n}$$，但用另一个内积：若$$c_{1},\cdots,c_{n}$$是正数，定义内积$$\langle(w_{1},\cdots,w_{n}),(z_{1},\cdots,z_{n})\rangle=c_{1}w_{1}\overline{z_{1}}+\cdots+c_{n}w_{n}\overline{z_{n}}$$
  - 由系数在$$\mathbf{F}$$中次数不超过$$m$$的所有多项式组成的向量空间$$\mathcal{P}_{m}(\mathbf{F})$$，定义内积$$\langle p,q\rangle=\int_{0}^{1}p(x)\overline{q(x)}\mathrm{d}x$$

## 范数

- 范数定义为$$\| \pmb{v}\|=\sqrt{\langle\pmb{v},\pmb{v}\rangle}$$。性质：
  - $$\| \pmb{v}\|=0$$当且仅当$$\pmb{v}=0$$
  - 对所有$$a\in\mathbf{F}, \pmb{v}\in V$$都有$$\| a\pmb{v}\|=\vert a\vert\| \pmb{v}\|$$（两边平方即可证）
- 对于两个向量$$\pmb{u},\pmb{v}\in V$$，如果$$\langle\pmb{u},\pmb{v}\rangle=0$$，则称$$\pmb{u}$$和$$\pmb{v}$$是正交的
  - 注意次序无关紧要（由共轭对称性）
  - $$\pmb{0}$$正交于每个向量，且$$\pmb{0}$$是唯一正交于自身的向量。
- 6.3：勾股定理：如果$$\pmb{u},\pmb{v}$$是$$V$$中的正交向量，那么$$\| \pmb{u}+\pmb{v}\|^{2}=\|\pmb{u}\|^{2}+\|\pmb{v}\|^{2}$$。证明：对$$\|\pmb{u}+\pmb{v}\|^{2}$$展开即得。
  - 易证这个命题的逆命题在实内积空间中成立。
- 6.5：正交分解：设$$\pmb{u},\pmb{v}\in V$$，如果$$\pmb{v}\ne0$$，可以证明这个式子把$$\pmb{u}$$写成了$$\pmb{v}$$的标量倍加上一个正交于$$\pmb{v}$$的向量：

  $$
  \pmb{u}=\frac{\langle\pmb{u},\pmb{v}\rangle}{\|\pmb{v}\|^{2}}\pmb{v}+\left(\pmb{u}-\frac{\langle\pmb{u},\pmb{v}\rangle}{\|\pmb{v}\|^{2}}\pmb{v}\right)
  $$
- 6.6：柯西-施瓦茨不等式（Cauchy-Schwarz Inequality）：若$$\pmb{u},\pmb{v}\in V$$，则

  $$
  \vert \langle\pmb{u},\pmb{v}\rangle\vert\le\| \pmb{u}\|\| \pmb{v}\|
  $$

  而且其中的等号成立当且仅当$$\pmb{u},\pmb{v}$$之一是另一个的标量倍。
  - 证明：设$$\pmb{v}\ne\pmb{0}$$（否则等号成立），然后用上述正交分解。令$$\pmb{w}$$等于右端第二项，则由勾股定理有：

    $$
    \| \pmb{u}\|^{2}=\left\| \frac{\langle\pmb{u},\pmb{v}\rangle}{\| \pmb{v}\|^{2}}\pmb{v}\right\|^{2}+\| \pmb{w}\|^{2}=\frac{\vert\langle\pmb{u},\pmb{v}\rangle\vert^{2}}{\|\pmb{v}\|^{2}}+\|\pmb{w}\|^{2}\ge\frac{\vert \langle\pmb{u},\pmb{v}\rangle\vert^{2}}{\|\pmb{v}\|^{2}}
    $$

    其中第二个等号成立是由于上面范数的第二个性质。然后再移项取平方根即得该不等式。从上式知等号成立当且仅当$$\pmb{w}=\pmb{0}$$，而由6.5知这成立当且仅当$$\pmb{u}$$和$$\pmb{v}$$的一个是另一个的标量倍。证毕。
- 6.9：三角不等式（Triangle Inequality）：若$$\pmb{u},\pmb{v}\in V$$则$$\|\pmb{u}+\pmb{v}\|\le\pmb{u}+\pmb{v}$$，而且其中的等号成立当且仅当$$\pmb{u},\pmb{v}$$之一是另一个的非负标量（实数）倍。
  - 证明：展开可得：

    $$
    \|\pmb{u}+\pmb{v}\|=\|\pmb{u}\|^{2}+\|\pmb{v}\|^{2}+2\text{Re}\langle\pmb{u},\pmb{v}\rangle\le \|\pmb{u}\|^{2}+\|\pmb{v}\|^{2}+2\vert \langle\pmb{u},\pmb{v}\rangle\vert \le\|\pmb{u}\|^{2}+\|\pmb{v}\|^{2}+2\|\pmb{u}\|\|\pmb{v}\|=(\|\pmb{u}\|+\|\pmb{v}\|)^{2}
    $$

    根据6.6，第二个小于等于号中的等号成立当且仅当$$\pmb{u},\pmb{v}$$之一是另一个的标量倍，而当该条件成立时第一个小于等于号中的等号也成立（该条件是充分非必要，就是说第一个小于等于号中的等号成立还可能是由于其他原因，但这已经足够），即得。
- 6.14：平行四边形等式（Parallelogram Equality）：若$$\pmb{u},\pmb{v}\in V$$，则$$\|\pmb{u}+\pmb{v}\|^{2}+\|\pmb{u}-\pmb{v}\|^{2}=2(\|\pmb{u}\|^{2}+\|\pmb{v}\|^{2})$$。证明：直接展开即得。
  - 几何解释为：在任意平行四边形中，对角线长度的平方和等于四条边长度的平方和。

## 规范正交基

- 规范正交性：若一个向量组中的向量两两正交，并且每个向量的范数都是1，则称这个向量组是规范正交的。
- 6.15：如果$$V$$中的向量组$$(\pmb{e}_{1},\cdots,\pmb{e}_{m})$$是规范正交的，那么$$\|a_{1}\pmb{e}_{1}+\cdots+a_{m}\pmb{e}_{m}\|^{2}=\vert a_{1}\vert^{2}+\cdots+\vert a_{m}\vert^{2}$$，其中$$a_{1},\cdots,a_{m}\in\mathbf{F}$$。证明：反复使用勾股定理6.3以及上述范数的第二个性质即得。
- 6.16：推论：每个规范正交向量组都是线性无关的。证明：用线性无关的定义（若存在一组标量使该规范正交向量组和这组标量对应项的积的和为0，那么这组标量全为0），根据6.15即得。
- 6.17：定理：设$$(\pmb{e}_{1},\cdots,\pmb{e}_{n})$$是$$V$$的规范正交基，则对每个$$\pmb{v}\in V$$都有：
  - 6.18：$$\pmb{v}=\langle\pmb{v},\pmb{e}_{1}\rangle\pmb{e}_{1}+\cdots+\langle\pmb{v},\pmb{e}_{n}\rangle\pmb{e}_{n}$$，而且
  - 6.19：$$\|\pmb{v}\|^{2}=\vert \langle\pmb{v},\pmb{e}_{1}\rangle\vert^{2}+\cdots+\vert \langle\pmb{v},\pmb{e}_{n}\rangle\vert^{2}$$
  - 证明：对等式$$\pmb{v}=a_{1}\pmb{e}_{1}+\cdots+a_{n}\pmb{e}_{n}$$两端都与$$\pmb{e}_{j}$$做内积，即得$$\langle\pmb{v},\pmb{e}_{j}\rangle=a_{j}$$，即6.18成立。又由6.15即得6.19。
- 6.20：格拉姆-施密特过程（Gram-Schmidt procedure）：如果$$(\pmb{v}_{1},\cdots,\pmb{v}_{m})$$是$$V$$中的线性无关向量组，则$$V$$有规范正交向量组$$(\pmb{e}_{1},\cdots,\pmb{e}_{m})$$使得：

  $$
  \text{span}(\pmb{v}_{1},\cdots,\pmb{v}_{j})=\text{span}(\pmb{e}_{1},\cdots,\pmb{e}_{j}), j=1,\cdots, m
  $$

  - 证明：首先令$$\pmb{e}_{1}=\pmb{v}_{1}/\|\pmb{v}_{1}\|$$，这满足$$j=1$$的情况。然后归纳地选取$$\pmb{e}_{1},\cdots,\pmb{e}_{m}$$。假设$$j>1$$并且已经选取了规范正交组$$(\pmb{e}_{1},\cdots,\pmb{e}_{j-1})$$使得$$\text{span}(\pmb{v}_{1},\cdots,\pmb{v}_{j-1})=\text{span}(\pmb{e}_{1},\cdots,\pmb{e}_{j-1})$$。现在令

    $$
    \pmb{e}_{j}=\frac{\pmb{v}_{j}-\langle\pmb{v}_{j},\pmb{e}_{1}\rangle\pmb{e}_{1}-\cdots-\langle\pmb{v}_{j},\pmb{e}_{j-1}\rangle\pmb{e}_{j-1}}{\|\pmb{v}_{j}-\langle\pmb{v}_{j},\pmb{e}_{1}\rangle\pmb{e}_{1}-\cdots-\langle\pmb{v}_{j},\pmb{e}_{j-1}\rangle\pmb{e}_{j-1}\|}
    $$

    易知$$\pmb{v}_{j}\notin\text{span}(\pmb{e}_{1},\cdots,\pmb{e}_{j-1})$$，因此上式良定义（分母不为0）且$$\|\pmb{e}_{j}\|=1$$。设$$1\le k\le j$$，用上式将$$\langle\pmb{e}_{j},\pmb{e}_{k}\rangle$$展开即得其值为0，因此$$(\pmb{e}_{1},\cdots,\pmb{e}_{j})$$是规范正交组。由上式知$$\pmb{v}_{j}\in\text{span}(\pmb{e}_{1},\cdots,\pmb{e}_{j})$$，再根据归纳假设即得$$\text{span}(\pmb{v}_{1},\cdots,\pmb{v}_{j})\subset\text{span}(\pmb{e}_{1},\cdots,\pmb{e}_{j})$$，而由于这两个组都是线性无关的（后者根据6.16得到），因此上面两个子空间的维数都是$$j$$，从而一定相等。
- 6.24：推论：每个有限维内积空间都有规范正交基。证明：直接对该空间的一组基应用格拉姆-施密特过程。因为该空间是有限维的，因此过程会停止并会得到一个规范正交组，根据6.16即得。
- 6.25：推论：$$V$$中的每个规范正交向量组都可以扩充为$$V$$的规范正交基。证明：先根据2.12将该向量组扩充为$$V$$的基，然后应用格拉姆-施密特过程即得。
- 6.27：推论：设$$T\in\mathcal{L}(V)$$，如果$$T$$关于$$V$$的某个基具有上三角矩阵，那么$$T$$关于$$V$$的某个规范正交基也具有上三角矩阵。证明：对该具有上三角矩阵的基应用格拉姆-施密特过程得到一组规范正交基，根据5.12(C)知$$T$$关于该组基具有上三角矩阵。
- 6.28：推论：设$$V$$是复向量空间，并且$$T\in\mathcal{L}(V)$$，则$$T$$关于$$V$$的某个规范正交基具有上三角矩阵。证明：由5.13和6.27即得。

## 正交投影与极小化问题

- 正交补：如果$$U$$是$$V$$的子集，那么$$U$$的正交补（orthogonal complement）为：

  $$
  U^{\bot}=\{\pmb{v}\in V:\langle\pmb{v},\pmb{u}\rangle=0, u\in U\}
  $$

  性质：
  - $$U^{\bot}$$总是$$V$$的子空间
  - $$V^{\bot}=\{\pmb{0}\}$$
  - $$\{\pmb{0}\}^{\bot}=V$$
- 6.29：定理：如果$$U$$是$$V$$的子空间，那么$$V=U\oplus U^{\bot}$$
  - 证明：先证$$V=U+U^{\bot}$$，为此设$$\pmb{v}\in V$$且$$(\pmb{e}_{1},\cdots,\pmb{e}_{m})$$是$$U$$的一个规范正交基，则有

    $$
    \pmb{v}=\underbrace{\langle\pmb{v},\pmb{e}_{1}\rangle\pmb{e}_{1}+\cdots+\langle\pmb{v},\pmb{e}_{m}\rangle\pmb{e}_{m}}_{\pmb{u}}+\underbrace{\pmb{v}-\langle\pmb{v},\pmb{e}_{1}\rangle\pmb{e}_{1}-\cdots-\langle\pmb{v},\pmb{e}_{m}\rangle\pmb{e}_{m}}_{\pmb{w}}
    $$

    显然$$\pmb{u}\in U$$，易证对每个$$j$$都有$$\langle\pmb{w},\pmb{e}_{j}\rangle=0$$，因此$$\pmb{w}\in U^{\bot}$$，证得。再证该和是直和，为此只需证$$U\cap U^{\bot}=\{\pmb{0}\}$$，为此设$$\pmb{v}$$属于该交集，则它（包含于$$U$$）正交于$$U$$中的每个向量（包括$$\pmb{v}$$本身），从而$$\langle\pmb{v},\pmb{v}\rangle=0$$即$$\pmb{v}=\pmb{0}$$，即得。
- 6.33：推论：如果$$U$$是$$V$$的子空间，那么$$U=(U^{\bot})^{\bot}$$
  - 证明：采用证明集合相等的一般方法，即每个属于其中一个集合的元素都属于另一个。设$$\pmb{u}\in U$$，根据定义很容易证明$$\pmb{u}\in (U^{\bot})^{\bot}$$。要证另一个方向，设$$\pmb{v}\in(U^{\bot})^{\bot}$$，根据6.29可得$$\pmb{v}=\pmb{u}+\pmb{w},\pmb{u}\in U,\pmb{w}\in U^{\bot}$$，从而$$\pmb{v}-\pmb{u}\in U^{\bot}$$；又因为$$\pmb{v}\in(U^{\bot})^{\bot}$$，而根据上述另一个方向的证明有$$\pmb{u}\in(U^{\bot})^{\bot}$$，因此$$\pmb{v}-\pmb{u}\in(U^{\bot})^{\bot}$$，因此$$\pmb{v}-\pmb{u}\in U^{\bot}\cap(U^{\bot})^{\bot}$$，故$$\pmb{v}=\pmb{u}$$，于是$$\pmb{v}\in U$$，证毕。
- 根据6.29，每个$$\pmb{v}\in V$$都可以唯一写成$$\pmb{v}=\pmb{u}+\pmb{w},\pmb{u}\in U,\pmb{w}\in U^{\bot}$$。定义$$V$$上的算子$$P_{U}$$，称为$$V$$到$$U$$上的正交投影：对$$\pmb{v}\in V$$，定义$$P_{U}\pmb{v}$$为前述分解中的$$\pmb{u}$$。易证：
  - $$P_{U}\in\mathcal{L}(V)$$；
  - $$\text{range}P_{U}=U$$；
  - $$\text{null}P_{U}=U^{\bot}$$；
  - 对每个$$\pmb{v}\in V$$都有$$\pmb{v}-P_{U}\pmb{v}\in U^{\bot}$$
  - $$P_{U}^{2}=P_{U}$$；
  - 对每个$$\pmb{v}\in V$$都有$$\|P_{U}\pmb{v}\|\le\|\pmb{v}\|$$
- 6.35：根据6.29的证明所用的分解可知，如果$$(\pmb{e}_{1},\cdots,\pmb{e}_{m})$$是$$U$$的规范正交基，那么对于每个$$\pmb{v}\in V$$都有$$P_{U}\pmb{v}=\langle\pmb{v},\pmb{e}_{1}\rangle\pmb{e}_{1}+\cdots+\langle\pmb{v},\pmb{e}_{m}\rangle\pmb{e}_{m}$$
- 6.36：命题：设$$U$$是$$V$$的子空间并且$$\pmb{v}\in V$$，则$$\|\pmb{v}-P_{U}\pmb{v}\|\le\|\pmb{v}-\pmb{u}\|,\pmb{u}\in U$$。进一步，若上面等号成立，则$$\pmb{u}=P_{U}\pmb{v}$$
  - 证明：

    $$
    \|\pmb{v}-P_{U}\pmb{v}\|^{2}\le\|\pmb{v}-P_{U}\pmb{v}\|^{2}+\|P_{U}\pmb{v}-\pmb{u}\|^{2}=\|(\pmb{v}-P_{U}\pmb{v})+(P_{U}\pmb{v}-\pmb{u})\|^{2}=\|\pmb{v}-\pmb{u}\|^{2}
    $$

    其中第一个等号是根据勾股定理。即得。
  - 例子：如何找到一个次数不超过5的实系数多项式$$\pmb{u}$$使其在区间$$[-\pi,\pi]$$上尽量好地逼近$$\sin x$$，即$$\int_{-\pi}^{\pi}\vert\sin x-\pmb{u}(x)\vert^{2}\mathrm{d}x$$最小
    - 解：令$$C[-\pi,\pi]$$表示由$$[-\pi,\pi]$$上的连续实值函数组成的实向量空间（注意这个空间不是有限维的），并取内积为：

      $$
      \langle f,g\rangle=\int_{-\pi}^{\pi}f(x)g(x)\mathrm{d}x
      $$

      设$$\pmb{v}\in C[-\pi,\pi]$$是由$$\pmb{v}(x)=\sin x$$定义的函数，令$$U$$表示由次数不超过5的实系数多项式组成的$$C[-\pi,\pi]$$的子空间，则问题可以重述为：求$$\pmb{u}\in U$$使得$$\|\pmb{v}-\pmb{u}\|$$最小。根据6.36要求的$$\pmb{u}$$就是$$P_{U}\pmb{v}$$，而这可以先找$$U$$的一组规范正交基然后根据6.35来求，而规范正交基可以对$$U$$的基$$(1,x,x^{2},x^{3},x^{4},x^{5})$$应用格拉姆-施密特过程得到。
  - 通过6.29的证明知，其只用到$$U$$的维数的有限性，而无论$$V$$维数是否有限证明都成立。对$$P_{U}$$的定义和性质（包括6.35），以及6.36也是一样的。因此可知：无论$$V$$是否为有限维的，只要$$U$$是有限维的，就可以用上面讨论的过程来寻找$$\pmb{u}\in U$$使得$$\|\pmb{v}-\pmb{u}\|$$最小。

## 线性泛函与伴随

- $$V$$上的线性泛函（linear functional）是从$$V$$到$$\mathbf{F}$$的线性映射。
- 6.45：定理：设$$\varphi$$是$$V$$上的线性泛函，则存在唯一一个向量$$\pmb{v}\in V$$使得$$\varphi(\pmb{u})=\langle\pmb{u},\pmb{v}\rangle,\pmb{u}\in V$$
  - 证明：首先证明存在向量$$\pmb{v}\in V$$使得对每个$$\pmb{u}\in V$$都有$$\varphi(\pmb{u})=\langle\pmb{u},\pmb{v}\rangle$$。设$$(\pmb{e}_{1},\cdots,\pmb{e}_{n})$$是$$V$$的规范正交基，则对每个$$\pmb{u}\in V$$都有：

    $$
    \begin{array}{l}
      \varphi(\pmb{u})&=\varphi\left(\langle\pmb{u},\pmb{e}_{1}\rangle\pmb{e}_{1}+\cdots+\langle\pmb{u},\pmb{e}_{n}\rangle\pmb{e}_{n}\right) \\
      &=\langle\pmb{u},\pmb{e}_{1}\rangle\varphi(\pmb{e}_{1})+\cdots+\langle\pmb{u},\pmb{e}_{n}\rangle\varphi(\pmb{e}_{n}) \\
      &=\langle\pmb{u},\overline{\varphi(\pmb{e}_{1})}\pmb{e}_{1}+\cdots+\overline{\varphi(\pmb{e}_{n})}\pmb{e}_{n}\rangle \\
      &=\langle\pmb{u},\pmb{v}\rangle
    \end{array}
    $$

    现在来证明这样的$$\pmb{v}$$只有一个，否则假设$$\pmb{v}_{1},\pmb{v}_{2}$$对任意的$$\pmb{u}$$均满足条件，则取$$\pmb{u}=\pmb{v}_{1}-\pmb{v}_{2}$$，易证$$\pmb{v}_{1}=\pmb{v}_{2}$$。
- 伴随（adjoint）的定义：设$$V,W$$是$$\mathbf{F}$$上的有限维非零内积空间，设$$T\in\mathcal{L}(V,W)$$。给定$$\pmb{w}\in W$$，考虑$$V$$上将$$\pmb{v}\in V$$映成$$\langle T\pmb{v},\pmb{w}\rangle$$的线性泛函（易证这个线性泛函满足线性映射的定义，因此是良定义的），则$$T$$的伴随$$T^{*}$$是使得如下性质成立的从$$W$$到$$V$$的函数：$$T^{*}w$$是$$V$$中唯一一个满足条件$$\langle T\pmb{v},\pmb{w}\rangle=\langle\pmb{v},T^{*}\pmb{w}\rangle,\pmb{v}\in V$$的向量
- 伴随是线性映射：若$$T\in\mathcal{L}(V,W)$$，则$$T^{*}\in\mathcal{L}(V,W)$$。证明：根据线性映射的定义需要证明加性和齐性。
  - 加性：$$\langle\pmb{v},T^{*}(\pmb{w}_{1}+\pmb{w}_{2})\rangle=\langle T\pmb{v},\pmb{w}_{1}+\pmb{w}_{2}\rangle=\langle T\pmb{v},\pmb{w}_{1}\rangle+\langle T\pmb{v},\pmb{w}_{2}\rangle=\langle\pmb{v},T^{*}\pmb{w}_{1}\rangle+\langle\pmb{v},T^{*}\pmb{w}_{2}\rangle=\langle\pmb{v},T^{*}\pmb{w}_{1}+T^{*}\pmb{w}_{2}\rangle$$
  - 齐性可以类似地证明。
- 函数$$T\mapsto T^{*}$$具有如下性质（以下设$$S,T\in\mathcal{L}(V,W)$$）
  - 加性：$$(S+T)^{*}=S^{*}+T^{*}$$。为证此性质需要证明$$(S+T)^{*}(\pmb{w})=S^{*}\pmb{w}+T^{*}\pmb{w},\pmb{w}\in W$$，为此需要证明$$\langle\pmb{v},(S+T)^{*}\pmb{w}\rangle=\langle\pmb{v},S^{*}\pmb{w}+T^{*}\pmb{w}\rangle$$，而这通过伴随定义的式子、内积的性质以及线性变换的性质就能证得。
  - 共轭齐性：$$(aT)^{*}=\overline{a}T^{*}$$。证明思路同上。
  - 伴随的伴随：$$(T^{*})^{*}=T$$。证明思路同上，要用到内积的共轭对称性。
  - 恒等算子：$$I^{*}=I$$
  - 乘积：$$(ST)^{*}=T^{*}S^{*}$$。证明思路同上，要把$$S$$和$$T$$通过伴随的定义式做两次变换。

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
