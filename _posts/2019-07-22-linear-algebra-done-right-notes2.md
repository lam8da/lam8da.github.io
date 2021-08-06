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

- 实向量空间或复向量空间$$V$$上的内积（inner product）就是一个函数，它把$$V$$中元素的每个有序对$$(\boldsymbol{u},\boldsymbol{v})$$都映成一个数$$\langle\boldsymbol{u},\boldsymbol{v}\rangle\in\mathbf{F}$$，并且具有性质：
  - 正性：对所有$$\boldsymbol{v}\in V$$都有$$\langle\boldsymbol{v},\boldsymbol{v}\rangle\ge0$$（这意味着$$\langle\boldsymbol{v},\boldsymbol{v}\rangle$$是一个实数）
  - 定性：$$\langle\boldsymbol{v},\boldsymbol{v}\rangle=0$$当且仅当$$\boldsymbol{v}=0$$
  - 第一个位置的加性：对所有$$\boldsymbol{u},\boldsymbol{v},\boldsymbol{w}\in V$$都有$$\langle\boldsymbol{u}+\boldsymbol{v},\boldsymbol{w}\rangle=\langle\boldsymbol{u},\boldsymbol{w}\rangle+\langle\boldsymbol{v},\boldsymbol{w}\rangle$$
    - 通过这个和下一个性质可以证明第二个位置也具有加性，即：$$\langle\boldsymbol{u},\boldsymbol{v}+\boldsymbol{w}\rangle=\langle\boldsymbol{u},\boldsymbol{v}\rangle+\langle\boldsymbol{u},\boldsymbol{w}\rangle$$
    - 可证$$\langle\boldsymbol{0},\boldsymbol{w}\rangle=\langle\boldsymbol{w},\boldsymbol{0}\rangle=0,\boldsymbol{w}\in V$$
  - 第一个位置的齐性：对所有$$a\in\mathbf{F},\boldsymbol{v},\boldsymbol{w}\in V$$都有$$\langle a\boldsymbol{v},\boldsymbol{w}\rangle=a\langle\boldsymbol{v},\boldsymbol{w}\rangle$$
    - 通过这个和上一个性质可以证明第二个位置具有共轭齐性，即：$$\langle\boldsymbol{u},a\boldsymbol{v}\rangle=\overline{a}\langle\boldsymbol{u},\boldsymbol{v}\rangle$$
  - 共轭对称性：对所有$$\boldsymbol{v},\boldsymbol{w}\in V$$都有$$\langle\boldsymbol{v},\boldsymbol{w}\rangle=\overline{\langle\boldsymbol{w},\boldsymbol{v}\rangle}$$
    - 对实向量空间这等价于$$\langle\boldsymbol{v},\boldsymbol{w}\rangle=\langle\boldsymbol{w},\boldsymbol{v}\rangle$$
- 内积空间是带有内积的向量空间。例子：
  - $$\mathbf{F}^{n}$$，内积定义为$$\langle(w_{1},\cdots,w_{n}),(z_{1},\cdots,z_{n})\rangle=w_{1}\overline{z_{1}}+\cdots+w_{n}\overline{z_{n}}$$（这称为欧几里得内积）
  - 同样为$$\mathbf{F}^{n}$$，但用另一个内积：若$$c_{1},\cdots,c_{n}$$是正数，定义内积$$\langle(w_{1},\cdots,w_{n}),(z_{1},\cdots,z_{n})\rangle=c_{1}w_{1}\overline{z_{1}}+\cdots+c_{n}w_{n}\overline{z_{n}}$$
  - 由系数在$$\mathbf{F}$$中次数不超过$$m$$的所有多项式组成的向量空间$$\mathcal{P}_{m}(\mathbf{F})$$，定义内积$$\langle p,q\rangle=\int_{0}^{1}p(x)\overline{q(x)}\mathrm{d}x$$

## 范数

- 范数定义为$$\| \boldsymbol{v}\|=\sqrt{\langle\boldsymbol{v},\boldsymbol{v}\rangle}$$。性质：
  - $$\| \boldsymbol{v}\|=0$$当且仅当$$\boldsymbol{v}=0$$
  - 对所有$$a\in\mathbf{F}, \boldsymbol{v}\in V$$都有$$\| a\boldsymbol{v}\|=\vert a\vert\| \boldsymbol{v}\|$$（两边平方即可证）
- 对于两个向量$$\boldsymbol{u},\boldsymbol{v}\in V$$，如果$$\langle\boldsymbol{u},\boldsymbol{v}\rangle=0$$，则称$$\boldsymbol{u}$$和$$\boldsymbol{v}$$是正交的
  - 注意次序无关紧要（由共轭对称性）
  - $$\boldsymbol{0}$$正交于每个向量，且$$\boldsymbol{0}$$是唯一正交于自身的向量。
- 6.3：勾股定理：如果$$\boldsymbol{u},\boldsymbol{v}$$是$$V$$中的正交向量，那么$$\| \boldsymbol{u}+\boldsymbol{v}\|^{2}=\|\boldsymbol{u}\|^{2}+\|\boldsymbol{v}\|^{2}$$。证明：对$$\|\boldsymbol{u}+\boldsymbol{v}\|^{2}$$展开即得。
  - 易证这个命题的逆命题在实内积空间中成立。
- 6.5：正交分解：设$$\boldsymbol{u},\boldsymbol{v}\in V$$，如果$$\boldsymbol{v}\ne0$$，可以证明这个式子把$$\boldsymbol{u}$$写成了$$\boldsymbol{v}$$的标量倍加上一个正交于$$\boldsymbol{v}$$的向量：

  $$
  \boldsymbol{u}=\frac{\langle\boldsymbol{u},\boldsymbol{v}\rangle}{\|\boldsymbol{v}\|^{2}}\boldsymbol{v}+\left(\boldsymbol{u}-\frac{\langle\boldsymbol{u},\boldsymbol{v}\rangle}{\|\boldsymbol{v}\|^{2}}\boldsymbol{v}\right)
  $$
- 6.6：柯西-施瓦茨不等式（Cauchy-Schwarz Inequality）：若$$\boldsymbol{u},\boldsymbol{v}\in V$$，则

  $$
  \vert \langle\boldsymbol{u},\boldsymbol{v}\rangle\vert\le\| \boldsymbol{u}\|\| \boldsymbol{v}\|
  $$

  而且其中的等号成立当且仅当$$\boldsymbol{u},\boldsymbol{v}$$之一是另一个的标量倍。
  - 证明：设$$\boldsymbol{v}\ne\boldsymbol{0}$$（否则等号成立），然后用上述正交分解。令$$\boldsymbol{w}$$等于右端第二项，则由勾股定理有：

    $$
    \| \boldsymbol{u}\|^{2}=\left\| \frac{\langle\boldsymbol{u},\boldsymbol{v}\rangle}{\| \boldsymbol{v}\|^{2}}\boldsymbol{v}\right\|^{2}+\| \boldsymbol{w}\|^{2}=\frac{\vert\langle\boldsymbol{u},\boldsymbol{v}\rangle\vert^{2}}{\|\boldsymbol{v}\|^{2}}+\|\boldsymbol{w}\|^{2}\ge\frac{\vert \langle\boldsymbol{u},\boldsymbol{v}\rangle\vert^{2}}{\|\boldsymbol{v}\|^{2}}
    $$

    其中第二个等号成立是由于上面范数的第二个性质。然后再移项取平方根即得该不等式。从上式知等号成立当且仅当$$\boldsymbol{w}=\boldsymbol{0}$$，而由6.5知这成立当且仅当$$\boldsymbol{u}$$和$$\boldsymbol{v}$$的一个是另一个的标量倍。证毕。
- 6.9：三角不等式（Triangle Inequality）：若$$\boldsymbol{u},\boldsymbol{v}\in V$$则$$\|\boldsymbol{u}+\boldsymbol{v}\|\le\boldsymbol{u}+\boldsymbol{v}$$，而且其中的等号成立当且仅当$$\boldsymbol{u},\boldsymbol{v}$$之一是另一个的非负标量（实数）倍。
  - 证明：展开可得：

    $$
    \|\boldsymbol{u}+\boldsymbol{v}\|=\|\boldsymbol{u}\|^{2}+\|\boldsymbol{v}\|^{2}+2\text{Re}\langle\boldsymbol{u},\boldsymbol{v}\rangle\le \|\boldsymbol{u}\|^{2}+\|\boldsymbol{v}\|^{2}+2\vert \langle\boldsymbol{u},\boldsymbol{v}\rangle\vert \le\|\boldsymbol{u}\|^{2}+\|\boldsymbol{v}\|^{2}+2\|\boldsymbol{u}\|\|\boldsymbol{v}\|=(\|\boldsymbol{u}\|+\|\boldsymbol{v}\|)^{2}
    $$

    根据6.6，第二个小于等于号中的等号成立当且仅当$$\boldsymbol{u},\boldsymbol{v}$$之一是另一个的标量倍，而当该条件成立时第一个小于等于号中的等号也成立（该条件是充分非必要，就是说第一个小于等于号中的等号成立还可能是由于其他原因，但这已经足够），即得。
- 6.14：平行四边形等式（Parallelogram Equality）：若$$\boldsymbol{u},\boldsymbol{v}\in V$$，则$$\|\boldsymbol{u}+\boldsymbol{v}\|^{2}+\|\boldsymbol{u}-\boldsymbol{v}\|^{2}=2(\|\boldsymbol{u}\|^{2}+\|\boldsymbol{v}\|^{2})$$。证明：直接展开即得。
  - 几何解释为：在任意平行四边形中，对角线长度的平方和等于四条边长度的平方和。

## 规范正交基

- 规范正交性：若一个向量组中的向量两两正交，并且每个向量的范数都是1，则称这个向量组是规范正交的。
- 6.15：如果$$V$$中的向量组$$(\boldsymbol{e}_{1},\cdots,\boldsymbol{e}_{m})$$是规范正交的，那么$$\|a_{1}\boldsymbol{e}_{1}+\cdots+a_{m}\boldsymbol{e}_{m}\|^{2}=\vert a_{1}\vert^{2}+\cdots+\vert a_{m}\vert^{2}$$，其中$$a_{1},\cdots,a_{m}\in\mathbf{F}$$。证明：反复使用勾股定理6.3以及上述范数的第二个性质即得。
- 6.16：推论：每个规范正交向量组都是线性无关的。证明：用线性无关的定义（若存在一组标量使该规范正交向量组和这组标量对应项的积的和为0，那么这组标量全为0），根据6.15即得。
- 6.17：定理：设$$(\boldsymbol{e}_{1},\cdots,\boldsymbol{e}_{n})$$是$$V$$的规范正交基，则对每个$$\boldsymbol{v}\in V$$都有：
  - 6.18：$$\boldsymbol{v}=\langle\boldsymbol{v},\boldsymbol{e}_{1}\rangle\boldsymbol{e}_{1}+\cdots+\langle\boldsymbol{v},\boldsymbol{e}_{n}\rangle\boldsymbol{e}_{n}$$，而且
  - 6.19：$$\|\boldsymbol{v}\|^{2}=\vert \langle\boldsymbol{v},\boldsymbol{e}_{1}\rangle\vert^{2}+\cdots+\vert \langle\boldsymbol{v},\boldsymbol{e}_{n}\rangle\vert^{2}$$
  - 证明：对等式$$\boldsymbol{v}=a_{1}\boldsymbol{e}_{1}+\cdots+a_{n}\boldsymbol{e}_{n}$$两端都与$$\boldsymbol{e}_{j}$$做内积，即得$$\langle\boldsymbol{v},\boldsymbol{e}_{j}\rangle=a_{j}$$，即6.18成立。又由6.15即得6.19。
- 6.20：格拉姆-施密特过程（Gram-Schmidt procedure）：如果$$(\boldsymbol{v}_{1},\cdots,\boldsymbol{v}_{m})$$是$$V$$中的线性无关向量组，则$$V$$有规范正交向量组$$(\boldsymbol{e}_{1},\cdots,\boldsymbol{e}_{m})$$使得：

  $$
  \text{span}(\boldsymbol{v}_{1},\cdots,\boldsymbol{v}_{j})=\text{span}(\boldsymbol{e}_{1},\cdots,\boldsymbol{e}_{j}), j=1,\cdots, m
  $$

  - 证明：首先令$$\boldsymbol{e}_{1}=\boldsymbol{v}_{1}/\|\boldsymbol{v}_{1}\|$$，这满足$$j=1$$的情况。然后归纳地选取$$\boldsymbol{e}_{1},\cdots,\boldsymbol{e}_{m}$$。假设$$j>1$$并且已经选取了规范正交组$$(\boldsymbol{e}_{1},\cdots,\boldsymbol{e}_{j-1})$$使得$$\text{span}(\boldsymbol{v}_{1},\cdots,\boldsymbol{v}_{j-1})=\text{span}(\boldsymbol{e}_{1},\cdots,\boldsymbol{e}_{j-1})$$。现在令

    $$
    \boldsymbol{e}_{j}=\frac{\boldsymbol{v}_{j}-\langle\boldsymbol{v}_{j},\boldsymbol{e}_{1}\rangle\boldsymbol{e}_{1}-\cdots-\langle\boldsymbol{v}_{j},\boldsymbol{e}_{j-1}\rangle\boldsymbol{e}_{j-1}}{\|\boldsymbol{v}_{j}-\langle\boldsymbol{v}_{j},\boldsymbol{e}_{1}\rangle\boldsymbol{e}_{1}-\cdots-\langle\boldsymbol{v}_{j},\boldsymbol{e}_{j-1}\rangle\boldsymbol{e}_{j-1}\|}
    $$

    易知$$\boldsymbol{v}_{j}\notin\text{span}(\boldsymbol{e}_{1},\cdots,\boldsymbol{e}_{j-1})$$，因此上式良定义（分母不为0）且$$\|\boldsymbol{e}_{j}\|=1$$。设$$1\le k\le j$$，用上式将$$\langle\boldsymbol{e}_{j},\boldsymbol{e}_{k}\rangle$$展开即得其值为0，因此$$(\boldsymbol{e}_{1},\cdots,\boldsymbol{e}_{j})$$是规范正交组。由上式知$$\boldsymbol{v}_{j}\in\text{span}(\boldsymbol{e}_{1},\cdots,\boldsymbol{e}_{j})$$，再根据归纳假设即得$$\text{span}(\boldsymbol{v}_{1},\cdots,\boldsymbol{v}_{j})\subset\text{span}(\boldsymbol{e}_{1},\cdots,\boldsymbol{e}_{j})$$，而由于这两个组都是线性无关的（后者根据6.16得到），因此上面两个子空间的维数都是$$j$$，从而一定相等。
- 6.24：推论：每个有限维内积空间都有规范正交基。证明：直接对该空间的一组基应用格拉姆-施密特过程。因为该空间是有限维的，因此过程会停止并会得到一个规范正交组，根据6.16即得。
- 6.25：推论：$$V$$中的每个规范正交向量组都可以扩充为$$V$$的规范正交基。证明：先根据2.12将该向量组扩充为$$V$$的基，然后应用格拉姆-施密特过程即得。
- 6.27：推论：设$$T\in\mathcal{L}(V)$$，如果$$T$$关于$$V$$的某个基具有上三角矩阵，那么$$T$$关于$$V$$的某个规范正交基也具有上三角矩阵。证明：对该具有上三角矩阵的基应用格拉姆-施密特过程得到一组规范正交基，根据5.12(C)知$$T$$关于该组基具有上三角矩阵。
- 6.28：推论：设$$V$$是复向量空间，并且$$T\in\mathcal{L}(V)$$，则$$T$$关于$$V$$的某个规范正交基具有上三角矩阵。证明：由5.13和6.27即得。

## 正交投影与极小化问题

- 正交补：如果$$U$$是$$V$$的子集，那么$$U$$的正交补（orthogonal complement）为：

  $$
  U^{\bot}=\{\boldsymbol{v}\in V:\langle\boldsymbol{v},\boldsymbol{u}\rangle=0, u\in U\}
  $$

  性质：
  - $$U^{\bot}$$总是$$V$$的子空间
  - $$V^{\bot}=\{\boldsymbol{0}\}$$
  - $$\{\boldsymbol{0}\}^{\bot}=V$$
- 6.29：定理：如果$$U$$是$$V$$的子空间，那么$$V=U\oplus U^{\bot}$$
  - 证明：先证$$V=U+U^{\bot}$$，为此设$$\boldsymbol{v}\in V$$且$$(\boldsymbol{e}_{1},\cdots,\boldsymbol{e}_{m})$$是$$U$$的一个规范正交基，则有

    $$
    \boldsymbol{v}=\underbrace{\langle\boldsymbol{v},\boldsymbol{e}_{1}\rangle\boldsymbol{e}_{1}+\cdots+\langle\boldsymbol{v},\boldsymbol{e}_{m}\rangle\boldsymbol{e}_{m}}_{\boldsymbol{u}}+\underbrace{\boldsymbol{v}-\langle\boldsymbol{v},\boldsymbol{e}_{1}\rangle\boldsymbol{e}_{1}-\cdots-\langle\boldsymbol{v},\boldsymbol{e}_{m}\rangle\boldsymbol{e}_{m}}_{\boldsymbol{w}}
    $$

    显然$$\boldsymbol{u}\in U$$，易证对每个$$j$$都有$$\langle\boldsymbol{w},\boldsymbol{e}_{j}\rangle=0$$，因此$$\boldsymbol{w}\in U^{\bot}$$，证得。再证该和是直和，为此只需证$$U\cap U^{\bot}=\{\boldsymbol{0}\}$$，为此设$$\boldsymbol{v}$$属于该交集，则它（包含于$$U$$）正交于$$U$$中的每个向量（包括$$\boldsymbol{v}$$本身），从而$$\langle\boldsymbol{v},\boldsymbol{v}\rangle=0$$即$$\boldsymbol{v}=\boldsymbol{0}$$，即得。
- 6.33：推论：如果$$U$$是$$V$$的子空间，那么$$U=(U^{\bot})^{\bot}$$
  - 证明：采用证明集合相等的一般方法，即每个属于其中一个集合的元素都属于另一个。设$$\boldsymbol{u}\in U$$，根据定义很容易证明$$\boldsymbol{u}\in (U^{\bot})^{\bot}$$。要证另一个方向，设$$\boldsymbol{v}\in(U^{\bot})^{\bot}$$，根据6.29可得$$\boldsymbol{v}=\boldsymbol{u}+\boldsymbol{w},\boldsymbol{u}\in U,\boldsymbol{w}\in U^{\bot}$$，从而$$\boldsymbol{v}-\boldsymbol{u}\in U^{\bot}$$；又因为$$\boldsymbol{v}\in(U^{\bot})^{\bot}$$，而根据上述另一个方向的证明有$$\boldsymbol{u}\in(U^{\bot})^{\bot}$$，因此$$\boldsymbol{v}-\boldsymbol{u}\in(U^{\bot})^{\bot}$$，因此$$\boldsymbol{v}-\boldsymbol{u}\in U^{\bot}\cap(U^{\bot})^{\bot}$$，故$$\boldsymbol{v}=\boldsymbol{u}$$，于是$$\boldsymbol{v}\in U$$，证毕。
- 根据6.29，每个$$\boldsymbol{v}\in V$$都可以唯一写成$$\boldsymbol{v}=\boldsymbol{u}+\boldsymbol{w},\boldsymbol{u}\in U,\boldsymbol{w}\in U^{\bot}$$。定义$$V$$上的算子$$P_{U}$$，称为$$V$$到$$U$$上的正交投影：对$$\boldsymbol{v}\in V$$，定义$$P_{U}\boldsymbol{v}$$为前述分解中的$$\boldsymbol{u}$$。易证：
  - $$P_{U}\in\mathcal{L}(V)$$；
  - $$\text{range}P_{U}=U$$；
  - $$\text{null}P_{U}=U^{\bot}$$；
  - 对每个$$\boldsymbol{v}\in V$$都有$$\boldsymbol{v}-P_{U}\boldsymbol{v}\in U^{\bot}$$
  - $$P_{U}^{2}=P_{U}$$；
  - 对每个$$\boldsymbol{v}\in V$$都有$$\|P_{U}\boldsymbol{v}\|\le\|\boldsymbol{v}\|$$
- 6.35：根据6.29的证明所用的分解可知，如果$$(\boldsymbol{e}_{1},\cdots,\boldsymbol{e}_{m})$$是$$U$$的规范正交基，那么对于每个$$\boldsymbol{v}\in V$$都有$$P_{U}\boldsymbol{v}=\langle\boldsymbol{v},\boldsymbol{e}_{1}\rangle\boldsymbol{e}_{1}+\cdots+\langle\boldsymbol{v},\boldsymbol{e}_{m}\rangle\boldsymbol{e}_{m}$$
- 6.36：命题：设$$U$$是$$V$$的子空间并且$$\boldsymbol{v}\in V$$，则$$\|\boldsymbol{v}-P_{U}\boldsymbol{v}\|\le\|\boldsymbol{v}-\boldsymbol{u}\|,\boldsymbol{u}\in U$$。进一步，若上面等号成立，则$$\boldsymbol{u}=P_{U}\boldsymbol{v}$$
  - 证明：

    $$
    \|\boldsymbol{v}-P_{U}\boldsymbol{v}\|^{2}\le\|\boldsymbol{v}-P_{U}\boldsymbol{v}\|^{2}+\|P_{U}\boldsymbol{v}-\boldsymbol{u}\|^{2}=\|(\boldsymbol{v}-P_{U}\boldsymbol{v})+(P_{U}\boldsymbol{v}-\boldsymbol{u})\|^{2}=\|\boldsymbol{v}-\boldsymbol{u}\|^{2}
    $$

    其中第一个等号是根据勾股定理。即得。
  - 例子：如何找到一个次数不超过5的实系数多项式$$\boldsymbol{u}$$使其在区间$$[-\pi,\pi]$$上尽量好地逼近$$\sin x$$，即$$\int_{-\pi}^{\pi}\vert\sin x-\boldsymbol{u}(x)\vert^{2}\mathrm{d}x$$最小
    - 解：令$$C[-\pi,\pi]$$表示由$$[-\pi,\pi]$$上的连续实值函数组成的实向量空间（注意这个空间不是有限维的），并取内积为：

      $$
      \langle f,g\rangle=\int_{-\pi}^{\pi}f(x)g(x)\mathrm{d}x
      $$

      设$$\boldsymbol{v}\in C[-\pi,\pi]$$是由$$\boldsymbol{v}(x)=\sin x$$定义的函数，令$$U$$表示由次数不超过5的实系数多项式组成的$$C[-\pi,\pi]$$的子空间，则问题可以重述为：求$$\boldsymbol{u}\in U$$使得$$\|\boldsymbol{v}-\boldsymbol{u}\|$$最小。根据6.36要求的$$\boldsymbol{u}$$就是$$P_{U}\boldsymbol{v}$$，而这可以先找$$U$$的一组规范正交基然后根据6.35来求，而规范正交基可以对$$U$$的基$$(1,x,x^{2},x^{3},x^{4},x^{5})$$应用格拉姆-施密特过程得到。
  - 通过6.29的证明知，其只用到$$U$$的维数的有限性，而无论$$V$$维数是否有限证明都成立。对$$P_{U}$$的定义和性质（包括6.35），以及6.36也是一样的。因此可知：无论$$V$$是否为有限维的，只要$$U$$是有限维的，就可以用上面讨论的过程来寻找$$\boldsymbol{u}\in U$$使得$$\|\boldsymbol{v}-\boldsymbol{u}\|$$最小。

## 线性泛函与伴随

- $$V$$上的线性泛函（linear functional）是从$$V$$到$$\mathbf{F}$$的线性映射。
- 6.45：定理：设$$\varphi$$是$$V$$上的线性泛函，则存在唯一一个向量$$\boldsymbol{v}\in V$$使得$$\varphi(\boldsymbol{u})=\langle\boldsymbol{u},\boldsymbol{v}\rangle,\boldsymbol{u}\in V$$
  - 证明：首先证明存在向量$$\boldsymbol{v}\in V$$使得对每个$$\boldsymbol{u}\in V$$都有$$\varphi(\boldsymbol{u})=\langle\boldsymbol{u},\boldsymbol{v}\rangle$$。设$$(\boldsymbol{e}_{1},\cdots,\boldsymbol{e}_{n})$$是$$V$$的规范正交基，则对每个$$\boldsymbol{u}\in V$$都有：

    $$
    \begin{array}{l}
      \varphi(\boldsymbol{u})&=\varphi\left(\langle\boldsymbol{u},\boldsymbol{e}_{1}\rangle\boldsymbol{e}_{1}+\cdots+\langle\boldsymbol{u},\boldsymbol{e}_{n}\rangle\boldsymbol{e}_{n}\right) \\
      &=\langle\boldsymbol{u},\boldsymbol{e}_{1}\rangle\varphi(\boldsymbol{e}_{1})+\cdots+\langle\boldsymbol{u},\boldsymbol{e}_{n}\rangle\varphi(\boldsymbol{e}_{n}) \\
      &=\langle\boldsymbol{u},\overline{\varphi(\boldsymbol{e}_{1})}\boldsymbol{e}_{1}+\cdots+\overline{\varphi(\boldsymbol{e}_{n})}\boldsymbol{e}_{n}\rangle \\
      &=\langle\boldsymbol{u},\boldsymbol{v}\rangle
    \end{array}
    $$

    现在来证明这样的$$\boldsymbol{v}$$只有一个，否则假设$$\boldsymbol{v}_{1},\boldsymbol{v}_{2}$$对任意的$$\boldsymbol{u}$$均满足条件，则取$$\boldsymbol{u}=\boldsymbol{v}_{1}-\boldsymbol{v}_{2}$$，易证$$\boldsymbol{v}_{1}=\boldsymbol{v}_{2}$$。
- 伴随（adjoint）的定义：设$$V,W$$是$$\mathbf{F}$$上的有限维非零内积空间，设$$T\in\mathcal{L}(V,W)$$。给定$$\boldsymbol{w}\in W$$，考虑$$V$$上将$$\boldsymbol{v}\in V$$映成$$\langle T\boldsymbol{v},\boldsymbol{w}\rangle$$的线性泛函（易证这个线性泛函满足线性映射的定义，因此是良定义的），则$$T$$的伴随$$T^{*}$$是使得如下性质成立的从$$W$$到$$V$$的函数：$$T^{*}w$$是$$V$$中唯一一个满足条件$$\langle T\boldsymbol{v},\boldsymbol{w}\rangle=\langle\boldsymbol{v},T^{*}\boldsymbol{w}\rangle,\boldsymbol{v}\in V$$的向量
- 伴随是线性映射：若$$T\in\mathcal{L}(V,W)$$，则$$T^{*}\in\mathcal{L}(V,W)$$。证明：根据线性映射的定义需要证明加性和齐性。
  - 加性：$$\langle\boldsymbol{v},T^{*}(\boldsymbol{w}_{1}+\boldsymbol{w}_{2})\rangle=\langle T\boldsymbol{v},\boldsymbol{w}_{1}+\boldsymbol{w}_{2}\rangle=\langle T\boldsymbol{v},\boldsymbol{w}_{1}\rangle+\langle T\boldsymbol{v},\boldsymbol{w}_{2}\rangle=\langle\boldsymbol{v},T^{*}\boldsymbol{w}_{1}\rangle+\langle\boldsymbol{v},T^{*}\boldsymbol{w}_{2}\rangle=\langle\boldsymbol{v},T^{*}\boldsymbol{w}_{1}+T^{*}\boldsymbol{w}_{2}\rangle$$
  - 齐性可以类似地证明。
- 函数$$T\mapsto T^{*}$$具有如下性质（以下设$$S,T\in\mathcal{L}(V,W)$$）
  - 加性：$$(S+T)^{*}=S^{*}+T^{*}$$。为证此性质需要证明$$(S+T)^{*}(\boldsymbol{w})=S^{*}\boldsymbol{w}+T^{*}\boldsymbol{w},\boldsymbol{w}\in W$$，为此需要证明$$\langle\boldsymbol{v},(S+T)^{*}\boldsymbol{w}\rangle=\langle\boldsymbol{v},S^{*}\boldsymbol{w}+T^{*}\boldsymbol{w}\rangle$$，而这通过伴随定义的式子、内积的性质以及线性变换的性质就能证得。
  - 共轭齐性：$$(aT)^{*}=\overline{a}T^{*}$$。证明思路同上。
  - 伴随的伴随：$$(T^{*})^{*}=T$$。证明思路同上，要用到内积的共轭对称性。
  - 恒等算子：$$I^{*}=I$$
  - 乘积：$$(ST)^{*}=T^{*}S^{*}$$。证明思路同上，要把$$S$$和$$T$$通过伴随的定义式做两次变换。
- 6.46：命题：设$$T\in\mathcal{L}(V,W)$$，则
  - (a)$$\text{null}T^{*}=(\text{range}T)^{\bot}$$
  - (b)$$\text{range}T^{*}=(\text{null}T)^{\bot}$$
  - (c)$$\text{null}T=(\text{range}T^{*})^{\bot}$$
  - (d)$$\text{range}T=(\text{null}T^{*})^{\bot}$$
  - 证明：对于(a)，设$$\boldsymbol{w}\in W$$，则：

    $$
    \begin{array}{l}
    \boldsymbol{w}\in\text{null}T^{*}&\iff T^{*}\boldsymbol{w}=\boldsymbol{0} \\
    &\iff \langle\boldsymbol{v},T^{*}\boldsymbol{w}\rangle=0,\boldsymbol{v}\in V \\
    &\iff \langle T\boldsymbol{v},\boldsymbol{w}\rangle=0,\boldsymbol{v}\in V \\
    &\iff \boldsymbol{w}\in (\text{range}T)^{\bot}
    \end{array}
    $$

    (a)的两端取正交补即得(d)（用到了6.33）。通过把这两式中的$$T$$和$$T^{*}$$互换即得另外两式。
- 6.47：命题：设$$T\in\mathcal{L}(V,W)$$，如果$$(\boldsymbol{e}_{1},\cdots,\boldsymbol{e}_{n})$$是$$V$$的规范正交基，且$$(\boldsymbol{f}_{1},\cdots,\boldsymbol{f}_{m})$$是$$W$$的规范正交基，那么

  $$
  \mathcal{M}(T^{*},(\boldsymbol{f}_{1},\cdots,\boldsymbol{f}_{m}),(\boldsymbol{e}_{1},\cdots,\boldsymbol{e}_{n}))
  $$

  是

  $$
  \mathcal{M}(T,(\boldsymbol{e}_{1},\cdots,\boldsymbol{e}_{n}),(\boldsymbol{f}_{1},\cdots,\boldsymbol{f}_{m}))
  $$

  的共轭装置。

# Helpers

```
a T in L(V,W): T\in\mathcal{L}(V)
b       <a,b>: \langle\boldsymbol{u},\boldsymbol{v}\rangle
e      rangeT: \text{range}T
f       nullT: \text{null}T
p            : p(z)=a_{0}+a_{1}z+a_{2}z^{2}+\cdots+a_{m}z^{m}
l   lambda1-m: \lambda_{1},\cdots,\lambda_{m}
u       u1-um: (\boldsymbol{u}_{1},\cdots,\boldsymbol{u}_{m})
v       v1-vn: (\boldsymbol{v}_{1},\cdots,\boldsymbol{v}_{n})
w       w1-wm: (\boldsymbol{w}_{1},\cdots,\boldsymbol{w}_{m})
```

`\mathbb is `$$\mathbb{abcdeABCDEwxyzWXYZ01234567890\alpha\beta\gamma\pi\phi\theta\Gamma\Pi\Phi\omega\Sigma\sigma\omega}$$

`\mathbf is `$$\mathbf{abcdeABCDEwxyzWXYZ01234567890\alpha\beta\gamma\pi\phi\theta\Gamma\Pi\Phi\omega\Sigma\sigma\omega}$$

`\mathcal is `$$\mathcal{abcdeABCDEwxyzWXYZ01234567890\alpha\beta\gamma\pi\phi\theta\Gamma\Pi\Phi\omega\Sigma\sigma\omega}$$

`\mathfrak is `$$\mathfrak{abcdeABCDEwxyzWXYZ01234567890\alpha\beta\gamma\pi\phi\theta\Gamma\Pi\Phi\omega\Sigma\sigma\omega}$$

`\mathit is `$$\mathit{abcdeABCDEwxyzWXYZ01234567890\alpha\beta\gamma\pi\phi\theta\Gamma\Pi\Phi\omega\Sigma\sigma\omega}$$

`\mathrm is `$$\mathrm{abcdeABCDEwxyzWXYZ01234567890\alpha\beta\gamma\pi\phi\theta\Gamma\Pi\Phi\omega\Sigma\sigma\omega}$$

`\mathscr is `$$\mathscr{abcdeABCDEwxyzWXYZ01234567890\alpha\beta\gamma\pi\phi\theta\Gamma\Pi\Phi\omega\Sigma\sigma\omega}$$

`\mathsf is `$$\mathsf{abcdeABCDEwxyzWXYZ01234567890\alpha\beta\gamma\pi\phi\theta\Gamma\Pi\Phi\omega\Sigma\sigma\omega}$$

`\mathtt is `$$\mathtt{abcdeABCDEwxyzWXYZ01234567890\alpha\beta\gamma\pi\phi\theta\Gamma\Pi\Phi\omega\Sigma\sigma\omega}$$

`\text is `$$\text{abcdeABCDEwxyzWXYZ01234567890\alpha\beta\gamma\pi\phi\theta\Gamma\Pi\Phi\omega\Sigma\sigma\omega}$$

`\textbf is `$$\textbf{abcdeABCDEwxyzWXYZ01234567890\alpha\beta\gamma\pi\phi\theta\Gamma\Pi\Phi\omega\Sigma\sigma\omega}$$

`\pmb is `$$\pmb{abcdeABCDEwxyzWXYZ01234567890\alpha\beta\gamma\pi\phi\theta\Gamma\Pi\Phi\omega\Sigma\sigma\omega}$$

`\boldsymbol is `$$\boldsymbol{abcdeABCDEwxyzWXYZ01234567890\alpha\beta\gamma\pi\phi\theta\Gamma\Pi\Phi\omega\Sigma\sigma\omega}$$
