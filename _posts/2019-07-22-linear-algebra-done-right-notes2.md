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

# 第六章 内积空间

向量空间研究的主要是空间的线性结构（加法和标量乘法），而内积空间则加上了对应于二维空间和三维空间中长度和角度的概念。

## 内积

- 实向量空间或复向量空间$$V$$上的内积（inner product）就是一个函数，它把$$V$$中元素的每个有序对$$(\vec{u},\vec{v})$$都映成一个数$$\viprod{u}{v}\in\mathbf{F}$$，并且具有性质：
  - 正性：对所有$$\vec{v}\in V$$都有$$\viprod{v}{v}\ge0$$（这意味着$$\viprod{v}{v}$$是一个实数）
  - 定性：$$\viprod{v}{v}=0$$当且仅当$$\vec{v}=0$$
  - 第一个位置的加性：对所有$$\vec{u},\vec{v},\vec{w}\in V$$都有$$\iprod{\vec{u}+\vec{v}}{\vec{w}}=\viprod{u}{w}+\viprod{v}{w}$$
    - 通过这个和下一个性质可以证明第二个位置也具有加性，即：$$\iprod{\vec{u}}{\vec{v}+\vec{w}}=\viprod{u}{v}+\viprod{u}{w}$$
    - 可证$$\viprod{0}{w}=\viprod{w}{0}=0,\vec{w}\in V$$
  - 第一个位置的齐性：对所有$$a\in\mathbf{F},\vec{v},\vec{w}\in V$$都有$$\iprod{ a\vec{v}}{\vec{w}}=a\viprod{v}{w}$$
    - 通过这个和上一个性质可以证明第二个位置具有共轭齐性，即：$$\iprod{\vec{u}}{a\vec{v}}=\overline{a}\viprod{u}{v}$$
  - 共轭对称性：对所有$$\vec{v},\vec{w}\in V$$都有$$\viprod{v}{w}=\overline{\viprod{w}{v}}$$
    - 对实向量空间这等价于$$\viprod{v}{w}=\viprod{w}{v}$$
- 内积空间是带有内积的向量空间。例子：
  - $$\mathbf{F}^{n}$$，内积定义为$$\iprod{(w_{1},\cdots,w_{n})}{(z_{1},\cdots,z_{n})}=w_{1}\overline{z_{1}}+\cdots+w_{n}\overline{z_{n}}$$（这称为欧几里得内积）
  - 同样为$$\mathbf{F}^{n}$$，但用另一个内积：若$$c_{1},\cdots,c_{n}$$是正数，定义内积$$\iprod{(w_{1},\cdots,w_{n})}{(z_{1},\cdots,z_{n})}=c_{1}w_{1}\overline{z_{1}}+\cdots+c_{n}w_{n}\overline{z_{n}}$$
  - 由系数在$$\mathbf{F}$$中次数不超过$$m$$的所有多项式组成的向量空间$$\mathcal{P}_{m}(\mathbf{F})$$，定义内积$$\iprod{ p}{q}=\int_{0}^{1}p(x)\overline{q(x)}\mathrm{d}x$$

## 范数

- 范数定义为$$\| \vec{v}\|=\sqrt{\viprod{v}{v}}$$。性质：
  - $$\| \vec{v}\|=0$$当且仅当$$\vec{v}=0$$
  - 对所有$$a\in\mathbf{F}, \vec{v}\in V$$都有$$\| a\vec{v}\|=\vert a\vert\| \vec{v}\|$$（两边平方即可证）
- 对于两个向量$$\vec{u},\vec{v}\in V$$，如果$$\viprod{u}{v}=0$$，则称$$\vec{u}$$和$$\vec{v}$$是正交的
  - 注意次序无关紧要（由共轭对称性）
  - $$\vec{0}$$正交于每个向量，且$$\vec{0}$$是唯一正交于自身的向量。
- 6.3：勾股定理：如果$$\vec{u},\vec{v}$$是$$V$$中的正交向量，那么$$\| \vec{u}+\vec{v}\|^{2}=\|\vec{u}\|^{2}+\|\vec{v}\|^{2}$$。证明：对$$\|\vec{u}+\vec{v}\|^{2}$$展开即得。
  - 易证这个命题的逆命题在实内积空间中成立。
- 6.5：正交分解：设$$\vec{u},\vec{v}\in V$$，如果$$\vec{v}\ne0$$，可以证明这个式子把$$\vec{u}$$写成了$$\vec{v}$$的标量倍加上一个正交于$$\vec{v}$$的向量：

  $$
  \vec{u}=\frac{\viprod{u}{v}}{\|\vec{v}\|^{2}}\vec{v}+\left(\vec{u}-\frac{\viprod{u}{v}}{\|\vec{v}\|^{2}}\vec{v}\right)
  $$
- 6.6：柯西-施瓦茨不等式（Cauchy-Schwarz Inequality）：若$$\vec{u},\vec{v}\in V$$，则

  $$
  \vert \viprod{u}{v}\vert\le\| \vec{u}\|\| \vec{v}\|
  $$

  而且其中的等号成立当且仅当$$\vec{u},\vec{v}$$之一是另一个的标量倍。
  - 证明：设$$\vec{v}\ne\vec{0}$$（否则等号成立），然后用上述正交分解。令$$\vec{w}$$等于右端第二项，则由勾股定理有：

    $$
    \| \vec{u}\|^{2}=\left\| \frac{\viprod{u}{v}}{\| \vec{v}\|^{2}}\vec{v}\right\|^{2}+\| \vec{w}\|^{2}=\frac{\vert\viprod{u}{v}\vert^{2}}{\|\vec{v}\|^{2}}+\|\vec{w}\|^{2}\ge\frac{\vert \viprod{u}{v}\vert^{2}}{\|\vec{v}\|^{2}}
    $$

    其中第二个等号成立是由于上面范数的第二个性质。然后再移项取平方根即得该不等式。从上式知等号成立当且仅当$$\vec{w}=\vec{0}$$，而由6.5知这成立当且仅当$$\vec{u}$$和$$\vec{v}$$的一个是另一个的标量倍。证毕。
- 6.9：三角不等式（Triangle Inequality）：若$$\vec{u},\vec{v}\in V$$则$$\|\vec{u}+\vec{v}\|\le\vec{u}+\vec{v}$$，而且其中的等号成立当且仅当$$\vec{u},\vec{v}$$之一是另一个的非负标量（实数）倍。
  - 证明：展开可得：

    $$
    \|\vec{u}+\vec{v}\|=\|\vec{u}\|^{2}+\|\vec{v}\|^{2}+2\text{Re}\viprod{u}{v}\le \|\vec{u}\|^{2}+\|\vec{v}\|^{2}+2\vert \viprod{u}{v}\vert \le\|\vec{u}\|^{2}+\|\vec{v}\|^{2}+2\|\vec{u}\|\|\vec{v}\|=(\|\vec{u}\|+\|\vec{v}\|)^{2}
    $$

    根据6.6，第二个小于等于号中的等号成立当且仅当$$\vec{u},\vec{v}$$之一是另一个的标量倍，而当该条件成立时第一个小于等于号中的等号也成立（该条件是充分非必要，就是说第一个小于等于号中的等号成立还可能是由于其他原因，但这已经足够），即得。
- 6.14：平行四边形等式（Parallelogram Equality）：若$$\vec{u},\vec{v}\in V$$，则$$\|\vec{u}+\vec{v}\|^{2}+\|\vec{u}-\vec{v}\|^{2}=2(\|\vec{u}\|^{2}+\|\vec{v}\|^{2})$$。证明：直接展开即得。
  - 几何解释为：在任意平行四边形中，对角线长度的平方和等于四条边长度的平方和。

## 规范正交基

- 规范正交性：若一个向量组中的向量两两正交，并且每个向量的范数都是1，则称这个向量组是规范正交的。
- 6.15：如果$$V$$中的向量组$$(\vlist{e}{m})$$是规范正交的，那么$$\|a_{1}\vec{e}_{1}+\cdots+a_{m}\vec{e}_{m}\|^{2}=\vert a_{1}\vert^{2}+\cdots+\vert a_{m}\vert^{2}$$，其中$$a_{1},\cdots,a_{m}\in\mathbf{F}$$。证明：反复使用勾股定理6.3以及上述范数的第二个性质即得。
- 6.16：推论：每个规范正交向量组都是线性无关的。证明：用线性无关的定义（若存在一组标量使该规范正交向量组和这组标量对应项的积的和为0，那么这组标量全为0），根据6.15即得。
- 6.17：定理：设$$(\vlist{e}{n})$$是$$V$$的规范正交基，则对每个$$\vec{v}\in V$$都有：
  - 6.18：$$\vec{v}=\iprod{\vec{v}}{\vec{e}_{1}}\vec{e}_{1}+\cdots+\iprod{\vec{v}}{\vec{e}_{n}}\vec{e}_{n}$$，而且
  - 6.19：$$\|\vec{v}\|^{2}=\vert \iprod{\vec{v}}{\vec{e}_{1}}\vert^{2}+\cdots+\vert \iprod{\vec{v}}{\vec{e}_{n}}\vert^{2}$$
  - 证明：对等式$$\vec{v}=a_{1}\vec{e}_{1}+\cdots+a_{n}\vec{e}_{n}$$两端都与$$\vec{e}_{j}$$做内积，即得$$\iprod{\vec{v}}{\vec{e}_{j}}=a_{j}$$，即6.18成立。又由6.15即得6.19。
- 6.20：格拉姆-施密特过程（Gram-Schmidt procedure）：如果$$(\vlist{v}{m})$$是$$V$$中的线性无关向量组，则$$V$$有规范正交向量组$$(\vlist{e}{m})$$使得：

  $$
  \vspanl{v}{j}=\vspanl{e}{j}, j=1,\cdots, m
  $$

  - 证明：首先令$$\vec{e}_{1}=\vec{v}_{1}/\|\vec{v}_{1}\|$$，这满足$$j=1$$的情况。然后归纳地选取$$\vlist{e}{m}$$。假设$$j>1$$并且已经选取了规范正交组$$(\vlist{e}{j-1})$$使得$$\vspanl{v}{j-1}=\vspanl{e}{j-1}$$。现在令

    $$
    \vec{e}_{j}=\frac{\vec{v}_{j}-\iprod{\vec{v}_{j}}{\vec{e}_{1}}\vec{e}_{1}-\cdots-\iprod{\vec{v}_{j}}{\vec{e}_{j-1}}\vec{e}_{j-1}}{\|\vec{v}_{j}-\iprod{\vec{v}_{j}}{\vec{e}_{1}}\vec{e}_{1}-\cdots-\iprod{\vec{v}_{j}}{\vec{e}_{j-1}}\vec{e}_{j-1}\|}
    $$

    易知$$\vec{v}_{j}\notin\vspanl{e}{j-1}$$，因此上式良定义（分母不为0）且$$\|\vec{e}_{j}\|=1$$。设$$1\le k\le j$$，用上式将$$\iprod{\vec{e}_{j}}{\vec{e}_{k}}$$展开即得其值为0，因此$$(\vlist{e}{j})$$是规范正交组。由上式知$$\vec{v}_{j}\in\vspanl{e}{j}$$，再根据归纳假设即得$$\vspanl{v}{j}\subset\vspanl{e}{j}$$，而由于这两个组都是线性无关的（后者根据6.16得到），因此上面两个子空间的维数都是$$j$$，从而一定相等。
- 6.24：推论：每个有限维内积空间都有规范正交基。证明：直接对该空间的一组基应用格拉姆-施密特过程。因为该空间是有限维的，因此过程会停止并会得到一个规范正交组，根据6.16即得。
- 6.25：推论：$$V$$中的每个规范正交向量组都可以扩充为$$V$$的规范正交基。证明：先根据2.12将该向量组扩充为$$V$$的基，然后应用格拉姆-施密特过程即得。
- 6.27：推论：设$$T\in\mathcal{L}(V)$$，如果$$T$$关于$$V$$的某个基具有上三角矩阵，那么$$T$$关于$$V$$的某个规范正交基也具有上三角矩阵。证明：对该具有上三角矩阵的基应用格拉姆-施密特过程得到一组规范正交基，根据5.12(C)知$$T$$关于该组基具有上三角矩阵。
- 6.28：推论：设$$V$$是复向量空间，并且$$T\in\mathcal{L}(V)$$，则$$T$$关于$$V$$的某个规范正交基具有上三角矩阵。证明：由5.13和6.27即得。

## 正交投影与极小化问题

- 正交补：如果$$U$$是$$V$$的子集，那么$$U$$的正交补（orthogonal complement）为：

  $$
  U^{\bot}=\{\vec{v}\in V:\viprod{v}{u}=0, u\in U\}
  $$

  性质：
  - $$U^{\bot}$$总是$$V$$的子空间
  - $$V^{\bot}=\{\vec{0}\}$$。
  - $$\{\vec{0}\}^{\bot}=V$$。
- 6.29：定理：如果$$U$$是$$V$$的子空间，那么$$V=U\oplus U^{\bot}$$
  - 证明：先证$$V=U+U^{\bot}$$，为此设$$\vec{v}\in V$$且$$(\vlist{e}{m})$$是$$U$$的一个规范正交基，则有

    $$
    \vec{v}=\underbrace{\iprod{\vec{v}}{\vec{e}_{1}}\vec{e}_{1}+\cdots+\iprod{\vec{v}}{\vec{e}_{m}}\vec{e}_{m}}_{\vec{u}}+\underbrace{\vec{v}-\iprod{\vec{v}}{\vec{e}_{1}}\vec{e}_{1}-\cdots-\iprod{\vec{v}}{\vec{e}_{m}}\vec{e}_{m}}_{\vec{w}}
    $$

    显然$$\vec{u}\in U$$，易证对每个$$j$$都有$$\iprod{\vec{w}}{\vec{e}_{j}}=0$$，因此$$\vec{w}\in U^{\bot}$$，证得。再证该和是直和，为此只需证$$U\cap U^{\bot}=\{\vec{0}\}$$，为此设$$\vec{v}$$属于该交集，则它（包含于$$U$$）正交于$$U$$中的每个向量（包括$$\vec{v}$$本身），从而$$\viprod{v}{v}=0$$即$$\vec{v}=\vec{0}$$，即得。
- 6.33：推论：如果$$U$$是$$V$$的子空间，那么$$U=(U^{\bot})^{\bot}$$
  - 证明：采用证明集合相等的一般方法，即每个属于其中一个集合的元素都属于另一个。设$$\vec{u}\in U$$，根据定义很容易证明$$\vec{u}\in (U^{\bot})^{\bot}$$。要证另一个方向，设$$\vec{v}\in(U^{\bot})^{\bot}$$，根据6.29可得$$\vec{v}=\vec{u}+\vec{w},\vec{u}\in U,\vec{w}\in U^{\bot}$$，从而$$\vec{v}-\vec{u}\in U^{\bot}$$；又因为$$\vec{v}\in(U^{\bot})^{\bot}$$，而根据上述另一个方向的证明有$$\vec{u}\in(U^{\bot})^{\bot}$$，因此$$\vec{v}-\vec{u}\in(U^{\bot})^{\bot}$$，因此$$\vec{v}-\vec{u}\in U^{\bot}\cap(U^{\bot})^{\bot}$$，故$$\vec{v}=\vec{u}$$，于是$$\vec{v}\in U$$，证毕。
- 根据6.29，每个$$\vec{v}\in V$$都可以唯一写成$$\vec{v}=\vec{u}+\vec{w},\vec{u}\in U,\vec{w}\in U^{\bot}$$。定义$$V$$上的算子$$P_{U}$$，称为$$V$$到$$U$$上的正交投影：对$$\vec{v}\in V$$，定义$$P_{U}\vec{v}$$为前述分解中的$$\vec{u}$$。易证：
  - $$P_{U}\in\mathcal{L}(V)$$；
  - $$\text{range}P_{U}=U$$；
  - $$\text{null}P_{U}=U^{\bot}$$；
  - 对每个$$\vec{v}\in V$$都有$$\vec{v}-P_{U}\vec{v}\in U^{\bot}$$
  - $$P_{U}^{2}=P_{U}$$；
  - 对每个$$\vec{v}\in V$$都有$$\|P_{U}\vec{v}\|\le\|\vec{v}\|$$
- 6.35：根据6.29的证明所用的分解可知，如果$$(\vlist{e}{m})$$是$$U$$的规范正交基，那么对于每个$$\vec{v}\in V$$都有$$P_{U}\vec{v}=\iprod{\vec{v}}{\vec{e}_{1}}\vec{e}_{1}+\cdots+\iprod{\vec{v}}{\vec{e}_{m}}\vec{e}_{m}$$
- 6.36：命题：设$$U$$是$$V$$的子空间并且$$\vec{v}\in V$$，则$$\|\vec{v}-P_{U}\vec{v}\|\le\|\vec{v}-\vec{u}\|,\vec{u}\in U$$。进一步，若上面等号成立，则$$\vec{u}=P_{U}\vec{v}$$
  - 证明：

    $$
    \|\vec{v}-P_{U}\vec{v}\|^{2}\le\|\vec{v}-P_{U}\vec{v}\|^{2}+\|P_{U}\vec{v}-\vec{u}\|^{2}=\|(\vec{v}-P_{U}\vec{v})+(P_{U}\vec{v}-\vec{u})\|^{2}=\|\vec{v}-\vec{u}\|^{2}
    $$

    其中第一个等号是根据勾股定理。即得。
  - 例子：如何找到一个次数不超过5的实系数多项式$$\vec{u}$$使其在区间$$[-\pi,\pi]$$上尽量好地逼近$$\sin x$$，即$$\int_{-\pi}^{\pi}\vert\sin x-\vec{u}(x)\vert^{2}\mathrm{d}x$$最小
    - 解：令$$C[-\pi,\pi]$$表示由$$[-\pi,\pi]$$上的连续实值函数组成的实向量空间（注意这个空间不是有限维的），并取内积为：

      $$
      \iprod{ f}{g}=\int_{-\pi}^{\pi}f(x)g(x)\mathrm{d}x
      $$

      设$$\vec{v}\in C[-\pi,\pi]$$是由$$\vec{v}(x)=\sin x$$定义的函数，令$$U$$表示由次数不超过5的实系数多项式组成的$$C[-\pi,\pi]$$的子空间，则问题可以重述为：求$$\vec{u}\in U$$使得$$\|\vec{v}-\vec{u}\|$$最小。根据6.36要求的$$\vec{u}$$就是$$P_{U}\vec{v}$$，而这可以先找$$U$$的一组规范正交基然后根据6.35来求，而规范正交基可以对$$U$$的基$$(1,x,x^{2},x^{3},x^{4},x^{5})$$应用格拉姆-施密特过程得到。
  - 通过6.29的证明知，其只用到$$U$$的维数的有限性，而无论$$V$$维数是否有限证明都成立。对$$P_{U}$$的定义和性质（包括6.35），以及6.36也是一样的。因此可知：无论$$V$$是否为有限维的，只要$$U$$是有限维的，就可以用上面讨论的过程来寻找$$\vec{u}\in U$$使得$$\|\vec{v}-\vec{u}\|$$最小。

## 线性泛函与伴随

- $$V$$上的线性泛函（linear functional）是从$$V$$到$$\mathbf{F}$$的线性映射。
- 6.45：定理：设$$\varphi$$是$$V$$上的线性泛函，则存在唯一一个向量$$\vec{v}\in V$$使得$$\varphi(\vec{u})=\viprod{u}{v},\vec{u}\in V$$
  - 证明：首先证明存在向量$$\vec{v}\in V$$使得对每个$$\vec{u}\in V$$都有$$\varphi(\vec{u})=\viprod{u}{v}$$。设$$(\vlist{e}{n})$$是$$V$$的规范正交基，则对每个$$\vec{u}\in V$$都有：

    $$
    \begin{array}{l}
      \varphi(\vec{u})&=\varphi\left(\iprod{\vec{u}}{\vec{e}_{1}}\vec{e}_{1}+\cdots+\iprod{\vec{u}}{\vec{e}_{n}}\vec{e}_{n}\right) \\
      &=\iprod{\vec{u}}{\vec{e}_{1}}\varphi(\vec{e}_{1})+\cdots+\iprod{\vec{u}}{\vec{e}_{n}}\varphi(\vec{e}_{n}) \\
      &=\iprod{\vec{u}}{\overline{\varphi(\vec{e}_{1})}\vec{e}_{1}+\cdots+\overline{\varphi(\vec{e}_{n})}\vec{e}_{n}} \\
      &=\viprod{u}{v}
    \end{array}
    $$

    现在来证明这样的$$\vec{v}$$只有一个，否则假设$$\vec{v}_{1},\vec{v}_{2}$$对任意的$$\vec{u}$$均满足条件，则取$$\vec{u}=\vec{v}_{1}-\vec{v}_{2}$$，易证$$\vec{v}_{1}=\vec{v}_{2}$$。
- 伴随（adjoint）的定义：设$$V,W$$是$$\mathbf{F}$$上的有限维非零内积空间，设$$T\in\mathcal{L}(V,W)$$。给定$$\vec{w}\in W$$，考虑$$V$$上将$$\vec{v}\in V$$映成$$\iprod{ T\vec{v}}{\vec{w}}$$的线性泛函$$\varphi_{\vec{w}}$$（易证这个线性泛函满足线性映射的定义，因此是良定义的），则$$T$$的伴随$$T^{*}$$是使得如下性质成立的从$$W$$到$$V$$的函数：$$T^{*}w$$是$$V$$中唯一一个满足条件$$\iprod{ T\vec{v}}{\vec{w}}=\iprod{\vec{v}}{T^{*}\vec{w}},\vec{v}\in V$$的向量（该向量的存在性和唯一性由6.45得到）
  - 注意：伴随是一个函数，其定义域为$$W$$，而上述描述定义了它在每个$$\vec{w}\in W$$上的值，该值满足：对任何$$\vec{v}\in V$$成立等式$$\iprod{ T\vec{v}}{\vec{w}}=\iprod{\vec{v}}{T^{*}\vec{w}}$$
- 伴随是线性映射：若$$T\in\mathcal{L}(V,W)$$，则$$T^{*}\in\mathcal{L}(V,W)$$。证明：根据线性映射的定义需要证明加性和齐性。
  - 加性：$$\iprod{\vec{v}}{T^{*}(\vec{w}_{1}+\vec{w}_{2})}=\iprod{ T\vec{v}}{\vec{w}_{1}+\vec{w}_{2}}=\iprod{ T\vec{v}}{\vec{w}_{1}}+\iprod{ T\vec{v}}{\vec{w}_{2}}=\iprod{\vec{v}}{T^{*}\vec{w}_{1}}+\iprod{\vec{v}}{T^{*}\vec{w}_{2}}=\iprod{\vec{v}}{T^{*}\vec{w}_{1}+T^{*}\vec{w}_{2}}$$
  - 齐性可以类似地证明。
- 函数$$T\mapsto T^{*}$$具有如下性质（以下设$$S,T\in\mathcal{L}(V,W)$$）
  - 加性：$$(S+T)^{*}=S^{*}+T^{*}$$。为证此性质需要证明$$(S+T)^{*}(\vec{w})=S^{*}\vec{w}+T^{*}\vec{w},\vec{w}\in W$$，为此需要证明$$\iprod{\vec{v}}{(S+T)^{*}\vec{w}}=\iprod{\vec{v}}{S^{*}\vec{w}+T^{*}\vec{w}}$$，而这通过伴随定义的式子、内积的性质以及线性变换的性质就能证得。
  - 共轭齐性：$$(aT)^{*}=\overline{a}T^{*}$$。证明思路同上。
  - 伴随的伴随：$$(T^{*})^{*}=T$$。证明思路同上，要用到内积的共轭对称性。
  - 恒等算子：$$I^{*}=I$$
  - 乘积：$$(ST)^{*}=T^{*}S^{*}$$。证明思路同上，要把$$S$$和$$T$$通过伴随的定义式做两次变换。
- 6.46：命题：设$$T\in\mathcal{L}(V,W)$$，则
  - (a)$$\text{null}T^{*}=(\text{range}T)^{\bot}$$
  - (b)$$\text{range}T^{*}=(\text{null}T)^{\bot}$$
  - (c)$$\text{null}T=(\text{range}T^{*})^{\bot}$$
  - (d)$$\text{range}T=(\text{null}T^{*})^{\bot}$$
  - 证明：对于(a)，设$$\vec{w}\in W$$，则：

    $$
    \begin{array}{l}
    \vec{w}\in\text{null}T^{*}&\iff T^{*}\vec{w}=\vec{0} \\
    &\iff \iprod{\vec{v}}{T^{*}\vec{w}}=0,\vec{v}\in V \\
    &\iff \iprod{ T\vec{v}}{\vec{w}}=0,\vec{v}\in V \\
    &\iff \vec{w}\in (\text{range}T)^{\bot}
    \end{array}
    $$

    (a)的两端取正交补即得(d)（用到了6.33）。通过把这两式中的$$T$$和$$T^{*}$$互换即得另外两式。
- 6.47：命题：设$$T\in\mathcal{L}(V,W)$$，如果$$(\vlist{e}{n})$$是$$V$$的规范正交基，且$$(\vlist{f}{m})$$是$$W$$的规范正交基，那么

  $$
  \mathcal{M}(T^{*},(\vlist{f}{m}),(\vlist{e}{n}))
  $$

  是

  $$
  \mathcal{M}(T,(\vlist{e}{n}),(\vlist{f}{m}))
  $$

  的共轭转置。
  - 证明：设$$(\vlist{e}{n}),(\vlist{f}{m})$$分别是$$V,W$$的规范正交基。我们知道，把$$T\vec{e}_{k}$$写成这些$$\vec{f}_{j}$$的线性组合可以得到$$\mathcal{M}(T)$$的第$$k$$列，而根据6.17有：

    $$
    T\vec{e}_{k}=\iprod{ T\vec{e}_{k}}{\vec{f}_{1}}\vec{f}_{1}+\cdots+\iprod{ T\vec{e}_{k}}{\vec{f}_{m}}\vec{f}_{m}
    $$

    因此$$\mathcal{M}(T)$$中第$$j$$行第$$k$$列的元素是$$\iprod{ T\vec{e}_{k}}{\vec{f}_{j}}$$。类似地$$\mathcal{M}(T^{*})$$中第$$j$$行第$$k$$列的元素是$$\iprod{ T^{*}\vec{f}_{k}}{\vec{e}_{j}}$$，而我们有：

    $$
    \iprod{ T^{*}\vec{f}_{k}}{\vec{e}_{j}}=\iprod{\vec{f}_{k}}{T\vec{e}_{j}}=\overline{\iprod{ T\vec{e}_{j}}{\vec{f}_{k}}}
    $$

    而这最后这值又等于$$\mathcal{M}(T)$$中第$$k$$行第$$j$$列元素的复共轭。证毕。
- 习题28：设$$T\in \mathcal{L}(V),\lambda\in\mathbf{F}$$。证明$$\lambda$$是$$T$$的本征值当且仅当$$\overline{\lambda}$$是$$T^{*}$$的本征值。
  > 如何证明？
    {: .lambda_question}
- 习题32：设$$\vec{A}$$为$$m\times n$$实矩阵。证明$$\vec{A}$$的所有列（在$$\mathbf{R}^{m}$$中）所张成的子空间的维数等于$$\vec{A}$$的所有行（在$$\mathbf{R}^{n}$$中）所张成的子空间的维数（即，行秩=列秩）。
  > 如何证明？
    {: .lambda_question}

# 第七章 内积空间上的算子

- 本章只讨论有限维非零内积空间
- 自伴（self-adjoint）算子：算子$$T\in\mathcal{L}(V)$$称为自伴的，如果$$T=T^{*}$$。易证：
  - 两个自伴算子的和是自伴的
  - 一个实数和一个自伴算子的乘积是自伴的
- 一些概念的梳理：
  - $$\mathcal{L}(V,W)$$ => $$\mathcal{L}(V)$$
  - 线性映射 => 算子
  - 伴随 => 自伴
- 重要的类比：伴随在$$\mathcal{L}(V)$$上所起的作用犹如复共轭在$$\mathbf{C}$$上所起的作用。复数$$z$$是实的当且仅当$$z=\overline{z}$$，因而自伴算子（$$T=T^{*}$$）可与实数类比。
- 7.1：命题：自伴算子的特征值都是实的。证明：思路就是证明$$\lambda=\overline{\lambda}$$：

  $$
  \begin{array}{l}
  \lambda\|\vec{v}\|^{2}=\iprod{\lambda\vec{v}}{\vec{v}}=\iprod{ T\vec{v}}{\vec{v}}=\iprod{\vec{v}}{T\vec{v}}=\iprod{\vec{v}}{\lambda\vec{v}}=\overline{\lambda}\|\vec{v}\|^{2}
  \end{array}
  $$
- 7.2：命题：若$$V$$是复内积空间，$$T$$是$$V$$上的算子，使得对所有$$\vec{v}\in V$$都有$$\iprod{ T\vec{v}}{\vec{v}}=0$$，则$$T=0$$
  - 证明：思路就是用$$\iprod{ T\vec{v}}{\vec{v}}$$的形式来表示$$\iprod{ T\vec{u}}{\vec{w}}$$：

    $$
    \begin{align}
      \iprod{ T\vec{u}}{\vec{w}} &=\frac{2\iprod{ T\vec{u}}{\vec{w}}+2\iprod{ T\vec{w}}{\vec{u}}}{4} + \frac{2\iprod{ T\vec{u}}{i\vec{w}}+2\iprod{ T(i\vec{w})}{\vec{u}}}{4}i \\
      &=\frac{\iprod{ T(\vec{u}+\vec{w})}{\vec{u}+\vec{w}}-\iprod{ T(\vec{u}-\vec{w})}{\vec{u}-\vec{w}}}{4}+\frac{\iprod{ T(\vec{u}+i\vec{w})}{\vec{u}+i\vec{w}}-\iprod{ T(\vec{u}-i\vec{w})}{\vec{u}-i\vec{w}}}{4}i \\
      &=0
    \end{align}
    $$

    从而$$T=0$$（取$$\vec{w}=T\vec{u}$$）。
  - 注意这个命题对实内积空间不成立，例如考虑算子$$T\in \mathcal{L}(\mathbf{R}^{2}),T((x,y))=(-y,x)$$（绕原点逆时针旋转90度）。
- 7.3：推论：设$$V$$是复内积空间，$$T\in\mathcal{L}(V)$$，则$$T$$是自伴的当且仅当对每个$$\vec{v}\in V$$都有$$\iprod{ T\vec{v}}{\vec{v}}\in \mathbf{R}$$
  - 证明：

    $$
    \begin{align}
      \iprod{ T\vec{v}}{\vec{v}}-\overline{\iprod{ T\vec{v}}{\vec{v}}}&=\iprod{ T\vec{v}}{\vec{v}}-\iprod{\vec{v}}{ T\vec{v}} \\
      &=\iprod{ T\vec{v}}{\vec{v}}-\iprod{ T^{*}\vec{v}}{\vec{v}} \\
      &=\iprod{(T-T^{*})\vec{v}}{\vec{v}}
    \end{align}
    $$

    - =>方向：若$$T$$是自伴的，则上式右端为0，从而左端的两者相等，即两者都为实数。
    - <=方向：若左端第一项为实数则左端为0，根据7.2知$$T-T^{*}=0$$，因此$$T$$是自伴的。
- 7.4：命题：若$$T$$是$$V$$上的自伴算子使得对所有$$\vec{v}\in V$$都有$$\iprod{ T\vec{v}}{\vec{v}}=0$$，则$$T=0$$
  - 证明：7.2已经证明了$$V$$是复内积空间的情况（且没有$$T$$是自伴的的假设），因此这里只需处理实内积空间的情况，这时有$$\iprod{ T\vec{w}}{\vec{u}}=\iprod{\vec{w}}{T\vec{u}}=\iprod{ T\vec{u}}{\vec{w}}$$，而据此可得：

    $$
    \begin{align}
      \iprod{ T\vec{u}}{\vec{w}} &=\frac{2\iprod{ T\vec{u}}{\vec{w}}+2\iprod{ T\vec{w}}{\vec{u}}}{4} \\
      &=\frac{\iprod{ T(\vec{u}+\vec{w})}{\vec{u}+\vec{w}}-\iprod{ T(\vec{u}-\vec{w})}{\vec{u}-\vec{w}}}{4} \\
      &=0
    \end{align}
    $$

    从而$$T=0$$（取$$\vec{w}=T\vec{u}$$）。
- 正规的（normal）：内积空间$$V$$上的算子$$T\in\mathcal{L}(V)$$称为正规的，如果$$TT^{*}=T^{*}T$$。
  - 自伴算子显然是正规的，反之不然。

# Helpers

```
a T in L(V,W): T\in\mathcal{L}(V)
b       <a,b>: \viprod{u}{v}
e      rangeT: \text{range}T
f       nullT: \text{null}T
p            : p(z)=a_{0}+a_{1}z+a_{2}z^{2}+\cdots+a_{m}z^{m}
l   lambda1-m: \lambda_{1},\cdots,\lambda_{m}
u       u1-um: (\vlist{u}{m})
v       v1-vn: (\vlist{v}{n})
w       w1-wm: (\vlist{w}{m})
```

`plain text: `$$abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ01234567890\alpha\beta\gamma\pi\phi\theta\lambda\sigma\omega\Gamma\Pi\Phi\Theta\Lambda\Sigma\Omega$$ 

`\mathbb is: `$$\mathbb{abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ01234567890\alpha\beta\gamma\pi\phi\theta\lambda\sigma\omega\Gamma\Pi\Phi\Theta\Lambda\Sigma\Omega}$$

`\mathbf is: `$$\mathbf{abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ01234567890\alpha\beta\gamma\pi\phi\theta\lambda\sigma\omega\Gamma\Pi\Phi\Theta\Lambda\Sigma\Omega}$$

`\mathcal is: `$$\mathcal{abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ01234567890\alpha\beta\gamma\pi\phi\theta\lambda\sigma\omega\Gamma\Pi\Phi\Theta\Lambda\Sigma\Omega}$$

`\mathfrak is: `$$\mathfrak{abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ01234567890\alpha\beta\gamma\pi\phi\theta\lambda\sigma\omega\Gamma\Pi\Phi\Theta\Lambda\Sigma\Omega}$$

`\mathit is: `$$\mathit{abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ01234567890\alpha\beta\gamma\pi\phi\theta\lambda\sigma\omega\Gamma\Pi\Phi\Theta\Lambda\Sigma\Omega}$$

`\mathrm is: `$$\mathrm{abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ01234567890\alpha\beta\gamma\pi\phi\theta\lambda\sigma\omega\Gamma\Pi\Phi\Theta\Lambda\Sigma\Omega}$$

`\mathscr is: `$$\mathscr{abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ01234567890\alpha\beta\gamma\pi\phi\theta\lambda\sigma\omega\Gamma\Pi\Phi\Theta\Lambda\Sigma\Omega}$$

`\mathsf is: `$$\mathsf{abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ01234567890\alpha\beta\gamma\pi\phi\theta\lambda\sigma\omega\Gamma\Pi\Phi\Theta\Lambda\Sigma\Omega}$$

`\mathtt is: `$$\mathtt{abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ01234567890\alpha\beta\gamma\pi\phi\theta\lambda\sigma\omega\Gamma\Pi\Phi\Theta\Lambda\Sigma\Omega}$$

`\text is: `$$\text{abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ01234567890\alpha\beta\gamma\pi\phi\theta\lambda\sigma\omega\Gamma\Pi\Phi\Theta\Lambda\Sigma\Omega}$$

`\textbf is: `$$\textbf{abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ01234567890\alpha\beta\gamma\pi\phi\theta\lambda\sigma\omega\Gamma\Pi\Phi\Theta\Lambda\Sigma\Omega}$$

`\pmb is: `$$\pmb{abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ01234567890\alpha\beta\gamma\pi\phi\theta\lambda\sigma\omega\Gamma\Pi\Phi\Theta\Lambda\Sigma\Omega}$$

`\boldsymbol is: `$$\vec{abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ01234567890\alpha\beta\gamma\pi\phi\theta\lambda\sigma\omega\Gamma\Pi\Phi\Theta\Lambda\Sigma\Omega}$$
