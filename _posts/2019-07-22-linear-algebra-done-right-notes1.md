---
layout: post
title: 《线性代数应该这样学》笔记（上）
date: 2019-07-22 22:04:32.601622
categories:
    - 数学&物理
tags:
    - 代数
    - 线性代数
---

{::comment} Not show line_numbers {:/comment}
{::options syntax_highlighter_opts="{default_lang: c++ \}" /}

* TOC
{:toc}

# 复数的一些性质

这里打乱顺序把关于复数的一些基本定义和性质单独列出，主要来自第一章和第四章第三节。

- 复数的定义：其实就是一个有序数对。$$\mathbf{C}$$上的加法和乘法满足：加法/乘法交换率、加法/乘法结合律、存在加法/乘法单位元（分别为0和1）、存在加法逆（和为0）和乘法逆（积为1）、分配率。
  - 乘法逆：由$$(a+bi)(a-bi)=a^{2}+b^{2}$$知道$$a+bi$$的逆为$$\frac{a-bi}{a^{2}+b^{2}}$$
- 复数$$z=a+bi$$，实部$$\text{Re }z$$，虚部$$\text{Im }z$$
- 复共轭定义为：$$\overline{z}=\text{Re }z-(\text{Im }z)i$$
- 绝对值定义为$$\vert z \vert=\sqrt{(\text{Re }z)^{2}+(\text{Im }z)^{2}}$$
- 一些性质：
  - $z\overline{z}=\vert z \vert^{2}$
  - $\overline{w+z}=\overline{w}+\overline{z}$
  - $\overline{wz}=\overline{w}\text{ }\overline{z}$
  - $\vert wz \vert=\vert w \vert\vert z \vert$（通过上述第一式和第三式得到）


# 第一章 向量空间

## 向量空间的定义

向量空间：就是定义了加法和（标量）乘法并满足以下性质的集合$$V$$：加法交换律、加法/乘法结合律、加法单位元（向量$$\vec{0}$$）、加法逆（和为向量$$\vec{0}$$）、乘法单位元（标量1）、分配率。其中$$V$$中的元素属于$$\mathbf{F}^n$$，而$$\mathbf{F}$$是$$\mathbf{R}$$或$$\mathbf{C}$$（以下均采用这个$$\mathbf{F}$$的定义）。

- 注意这里没有乘法交换律，因为这里乘法指标量乘法，而三个量间的乘法在有至少两个量是$$V$$中的元素时是无定义的，而如果有两个量是$$F$$中的元素则显然结果仍在$$V$$中。据此无法定义三个$$V$$中元素的乘法故无交换律。
- 向量空间的例子：
  - $$\mathbf{F}^n=\{(x_{1},\cdots,x_{n}): x_{j}\in \mathbf{F},j=1,\cdots,n\}$$；
  - $$\mathbf{F}^{\infty}=\{(x_{1},x_{2},\cdots):x_{j}\in F,j=1,2,\cdots\}$$；
  - 系数在$$\mathbf{F}$$中的所有多项式的集合：$$\mathcal{P}(\mathbf{F})$$
  - 系数在$$\mathbf{F}$$中并且次数不超过$$m$$的所有多项式组成的集合：$$\mathcal{P}_{m}(\mathbf{F})$$
- 易证：
  - 1.2：向量空间有唯一加法单位元
  - 1.3：向量空间中的每个元素都有唯一的加法逆
  - 1.4：对每个$$\vec{v}\in V$$都有$$0\vec{v}=\vec{0}$$
  - 1.5：对每个$$a\in\mathbf{F}$$都有$$a\vec{0}=\vec{0}$$
  - 1.6：对每个$$\vec{v}\in V$$都有$$(-1)\vec{v}=-\vec{v}$$

## 子空间

- $$V$$的子集$$U$$称为$$V$$的子空间，如果$$U$$（采用与$$V$$相同的加法和标量乘法）也是向量空间
- 如果$$U$$是$$V$$的子集，要验证$$U$$是$$V$$的子空间，只需验证$$U$$满足下列性质
  - 存在加法单位元$$\vec{0}$$
  - 对加法封闭
  - 对乘法封闭
- 例子：
  - $$\mathbf{R}^{2}$$的子空间恰为：$$\{\vec{0}\}$$，$$\mathbf{R}^{2}$$，$$\mathbf{R}^{2}$$中所有过原点的直线
  - $$\mathbf{R}^{3}$$的子空间恰为：$$\{\vec{0}\}$$，$$\mathbf{R}^{3}$$，$$\mathbf{R}^{3}$$中所有过原点的直线，$$\mathbf{R}^{3}$$中所有过原点的平面

## （空间的）和与直和

- 设$$U_{1},\cdots,U_{m}$$都是$$V$$的子空间，则它们的和定义为$$U_{1}+\cdots+U_{m}=\{\vec{u}_{1}+\cdots+\vec{u}_{m}:\vec{u}_{1}\in U_{1},\cdots,\vec{u}_{m}\in U_{m}\}$$
- 易证该和也是$$V$$的子空间，且是$$V$$中包含$$U_{1},\cdots,U_{m}$$的最小子空间
- 若$$U_{1},\cdots,U_{m}$$都是$$V$$的子空间且$$V=U_{1}+\cdots+U_{m}$$，而且$$V$$中每个元素都可以**唯一地**写成$$\vec{u}_{1}+\cdots+\vec{u}_{m}$$的形式，则称$$V$$是子空间$$U_{1},\cdots,U_{m}$$的直和（direct sum），记为$$V=U_{1}\oplus\cdots\oplus U_{m}$$
- 注意，如果$$V$$中每个元素都可以表示成$$\vec{u}_{1}+\cdots+\vec{u}_{m}$$的形式但表示方式不唯一，则不是直和
- 1.8：设$$U_{1},\cdots,U_{m}$$都是$$V$$的子空间，则$$V=U_{1}\oplus\cdots\oplus U_{m}$$当且仅当以下俩条件成立
  - 满足和的条件：$$V=U_{1}+\cdots+U_{m}$$
  - 若$$\vec{0}=\vec{u}_{1}+\cdots+\vec{u}_{m},\vec{u}_{j}\in U_{j}$$，则每个$$\vec{u}_{j}$$都为$$\vec{0}$$
- 1.9：是$$U$$和$$W$$都是$$V$$的子空间，则$$V=U\oplus W$$当且仅当$$V=U+W$$且$$U\cap W=\{\vec{0}\}$$

# 第二章 有限维向量空间

## （向量的）张成与线性无关

- $$V$$中的一组向量$$(\vec{v}_{1},\cdots,\vec{v}_{m})$$的所有线性组合构成的集合称为$$(\vec{v}_{1},\cdots,\vec{v}_{m})$$的张成（span），记为$$\vspanr{v}{m}=\{a_{1}\vec{v}_{1}+\cdots+a_{m}\vec{v}_{m}:a_{1},\cdots,a_{m}\in\mathbf{F}\}$$
  - 我们人为地令空组$$()$$的张成等于$$\{\vec{0}\}$$。这样，可以验证，$$V$$的任意一组向量的张成都是$$V$$的子空间，且是包含该组向量的最小子空间
  - 如果$$V=\vspanr{v}{m}$$，则称$$(\vec{v}_{1},\cdots,\vec{v}_{m})$$张成$$V$$。
  - 如果一个向量空间可以由它的一组（有限个）向量张成，则称其为有限维的（finite dimensional）（注意这里“有限”并不是指每个向量里的元素个数有限，例如$$\mathbf{F}_{x}^{\infty}=\{(x,x,\cdots),x\in \mathbf{F}\}$$是有限维（一维）的，但$$\mathbf{F}^{\infty}$$不是有限维的）
- 对于$$V$$中的一组向量$$(\vec{v}_{1},\cdots,\vec{v}_{m})$$，如果使得$$a_{1}\vec{v}_{1}+\cdots+a_{m}\vec{v}_{m}=0$$的$$a_{1},\cdots,a_{m}\in\mathbf{F}$$只有$$a_{1}=\cdots=a_{m}=0$$，则称$$(\vec{v}_{1},\cdots,\vec{v}_{m})$$是线性无关的（linearly independent）
  - 易证$$(\vec{v}_{1},\cdots,\vec{v}_{m})$$是线性无关的当且仅当$$\vspanr{v}{m}$$中的每个向量都可以唯一地表示成$$(\vec{v}_{1},\cdots,\vec{v}_{m})$$的线性组合
  - 我们人为地令空组$$()$$是线性无关的。这样可以证明，如果从一个线性无关向量组中去掉一些向量，那么余下的向量组还是线性无关的。
- 线性相关引理：如果$$(\vec{v}_{1},\cdots,\vec{v}_{m})$$在$$V$$中是线性相关的，并且$$\vec{v}_{1}\ne\vec{0}$$，则有$$j\in\{2,\cdots,m\}$$使得下列成立：
  - $$\vec{v}_{j}\in\vspanr{v}{m}$$（用张成的定义来证）
  - 如果从$$(\vec{v}_{1},\cdots,\vec{v}_{m})$$中去掉第$$j$$项，则剩余组的张成等于$$\vspanr{v}{m}$$
- 2.6定理：在有限维向量空间中，线性无关向量组的长度小于或等于张成向量组的长度。证明很不优美，要用上面的线性相关引理，从$$(\vec{w}_{1},\cdots,\vec{w}_{n})$$开始，每次往里面添加一个$$\vec{u}_{j}$$，然后根据线性相关引理可以删除一个$$\vec{w}$$，然后归纳法直至添加了所有的$$\vec{u}$$
- 2.7命题：有限维向量空间的子空间都是有限维的。证明：从$$\{\vec{0}\}$$开始，不断往里面添加不能由已有向量线性组合而成的向量，这个过程一定会停止，因为每次得到的组是一个线性无关组且其长度不会比$$V$$的任何张成组长（定理2.6）。

## 基

- 如果一个向量组既是线性无关的又张成$$V$$，则称之为$$V$$的基（basis）。如$$\mathbf{F}^{n}$$的一组基为$$((1,0,\cdots,0),(0,1,0,\cdots,0),\cdots,(0,\cdots,0,1))$$，称为其标准基（standard basis）
- 2.8命题：$$V$$中向量组$$(\vec{v}_{1},\cdots,\vec{v}_{m})$$是其基当且仅当每个$$\vec{v}\in V$$都能**唯一地**写成$$\vec{v}=a_{1}\vec{v}_{1}+\cdots+a_{m}\vec{v}_{m}$$，其中$$a_{1},\cdots,a_{m}\in\mathbf{F}$$
- 2.10定理：在向量空间中，每个张成组都可以简化成一个基。证明：不断尝试哪个元素可以被删掉而不影响张成，用线性相关引理即得。
- 2.11推论：每个有限维向量空间都有基。证明：根据上面对有限维的定义，每个有限维的向量空间都有张成组，从而由2.10即得。
- 2.12定理：在有限维向量空间中，每个线性无关向量组都可以扩充成一个基。证明：用张成该向量空间的任意一组向量的元素来不断填充该线性无关向量组直到成为基。
- 2.13命题：设$$V$$是有限维的，$$U$$是$$V$$的一个子空间，则存在$$V$$的一个子空间$$W$$使得$$V=U\oplus W$$。证明：把$$U$$的一个基扩充成$$V$$的基，扩充的部分那些向量的张成就是$$W$$（用1.9来证）。

## 维数

- 2.14定理：有限维向量空间的任意两个基的长度都相同。证明：对任意两个基，用基的定义并应用2.6即得（每个基的长度都小于等于另一个基的长度，因为基也是一个张成组）。
- 维数：定义为有限维向量空间的任意基的长度，记为$$\dim V$$
- 2.15定理：若$$V$$是有限维的，并且$$U$$是$$V$$的子空间，则$$\dim U\le\dim V$$。证明：用2.12。
- 2.16命题：若$$V$$是有限维的，则$$V$$中每个长度为$$\dim V$$的张成向量组都是$$V$$的一个基。证明：由2.10和2.14即得。
- 2.17命题：若$$V$$是有限维的，则$$V$$中每个长度为$$\dim V$$的线性无关向量组都是$$V$$的基。证明：由2.12和2.14即得。
- 2.18定理：如果$$U_{1}$$和$$U_{2}$$是同一个有限维向量空间的两个子空间，那么$$\dim(U_{1}+U_{2})=\dim U_{1}+\dim U_{2}-\dim(U_{1}\cap U_{2})$$。
  - 证明：设$$(\vec{u}_{1},\cdots,\vec{u}_{m})$$是$$U_{1}\cap U_{2}$$的基，它可以扩充为$$U_{1}$$的一个基$$(\vec{u}_{1},\cdots,\vec{u}_{m},\vec{v}_{1},\cdots,\vec{v}_{j})$$，也可以扩充为$$U_{2}$$的一组基$$(\vec{u}_{1},\cdots,\vec{u}_{m},\vec{w}_{1},\cdots,\vec{w}_{k})$$。只需证$$(\vec{u}_{1},\cdots,\vec{u}_{m},\vec{v}_{1},\cdots,\vec{v}_{j},\vec{w}_{1},\cdots,\vec{w}_{k})$$是$$U_{1}+U_{2}$$的一个基，而这只需证明该组是线性无关的，用定义以及$$U_{1}$$和$$U_{2}$$的关系即可证。
- 2.19命题：设$$V$$是有限维的，并且$$U_{1},\cdots,U_{m}$$是$$V$$的子空间，使得$$V=U_{1}+\cdots+U_{m}$$并且$$\dim V=\dim U_{1}+\cdots+\dim U_{m}$$，则$$V=U_{1}\oplus\cdots\oplus U_{m}$$。
  - 证明：根据假设，可以把每个$$U$$的基concat成一个长为$$\dim V$$的组，而且这个组张成$$V$$，从而是$$V$$的基（2.16）且线性无关。用1.8可以证得。

## 习题

- 习题16：证明：若$$V$$是有限维向量空间且$$U_{1},\cdots,U_{m}$$都是$$V$$的子空间，则$$\dim(U_{1}+\cdots+U_{m})\le\dim U_{1}+\cdots+\dim U_{m}$$。
  - 证明：对$$m$$进行归纳。$$m=2$$时由2.18即得。设当$$m=k$$时成立，则当$$m=k+1$$时有$$\dim(U_{1}+\cdots+U_{k}+U_{k+1})\le\dim(U_{1}+\cdots+U_{k})+\dim U_{k+1}\le\dim U_{1}+\cdots+\dim U_{k}+\dim U_{k+1}$$
- 习题17：设$$V$$是有限维的，证明：如果$$U_{1},\cdots,U_{m}$$是$$V$$的子空间使得$$V=U_{1}\oplus\cdots\oplus U_{m}$$，那么$$\dim V=\dim U_{1}+\cdots+\dim U_{m}$$。
  - 证明：设$$U_{k}$$的一组基为$$(\vec{v}_{1}^{k},\cdots,\vec{v}_{\dim U_{k}}^{k})$$，易知所有这些$$\vec{v}$$线性无关，否则$$\vec{w}=\sum_{k=1}^{m}\sum_{i=1}^{\dim U_{k}}\vec{v}_{i}^{k}$$就有多于一种的表示法，与直和的定义矛盾。又因为所有这些$$\vec{v}$$都属于$$V$$，因此它们可以扩充成$$V$$的基，故$$\dim V\ge\dim U_{1}+\cdots+\dim U_{m}$$。用习题16可以证明另一个方向，即得。

# 第三章 线性映射

## 定义

- 从$$V$$到$$W$$的线性映射（linear map）是具有下列性质的函数$$T:V\to W$$：
  - 加性：对所有$$\vec{u},\vec{v}\in V$$都有$$T(\vec{u}+\vec{b})=T\vec{u}+T\vec{v}$$
  - 齐性：对所有$$a\in\mathbf{F},\vec{v}\in V$$都有$$T(a\vec{v})=a(T\vec{v})$$

  > 问：把2D矩阵看成一个向量空间（可证明其具有向量空间定义所需性质），则在其上的Conv2D操作可看成一个线性映射（可证明Conv2D具有线性映射的所需性质）。那么，因此连续两个Conv2D可以合成为另一个单独的线性映射（通过矩阵乘法）？
    {: .lambda_question}
  
- 从$$V$$到$$W$$的所有线性映射所构成的集合记为$$\mathcal{L}(V,W)$$
- 线性映射的例子：
  - 零：把某个向量空间的每个元素都映成另一个向量空间的加法单位元
  - 恒等映射
  - 微分：把一个可微函数映成其导函数
  - 定积分：定义$$T\in\mathcal{L}(\mathcal{P}(\mathbf{R}),\mathbf{R})$$为$$T_{\vec{p}}=\int_{0}^{1}\vec{p}(x)\mathrm{d}x$$
  - 从$$\mathbf{F}^{n}$$到$$\mathbf{F}^{m}$$：设$$m$$和$$n$$都是正整数，$$a_{j,k}\in\mathbf{F},j=1,\cdots,m,k=1,\cdots,n$$，定义$$T\in\mathcal{L}(\mathbf{F}^{n},\mathbf{F}^{m})$$为

    $$
    T(x_{1},\cdots,x_{n})=(a_{1,1}x_{1}+\cdots+a_{1,n}x_{n},\cdots,a_{m,1}x_{1}+\cdots+a_{m,n}x_{n})
    $$

    后面会证明从$$\mathbf{F}^{n}$$到$$\mathbf{F}^{m}$$的每个线性映射都是这种形式的。
- 在$$\mathcal{L}(V,W)$$上定义加法和标量乘法使其成为一个向量空间。对于$$S,T\in\mathcal{L}(V,W)$$，可以定义
  - 加法：$$(S+T)\vec{v}=S\vec{v}+T\vec{v},\vec{v}\in V$$
  - 标量乘法：$$(aT)\vec{v}=a(T\vec{v}),\vec{v}\in V$$
  - 通过向量空间的定义可证
- 设$$U$$是$$\mathbf{F}$$上的向量空间，如果$$T\in\mathcal{L}(U,V),S\in\mathcal{L}(V,W)$$，那么定义$$ST\in\mathcal{L}(U,W)$$为$$(ST)(\vec{v})=S(T\vec{v}),\vec{v}\in U$$。可以验证此时$$ST$$的确是从$$U$$到$$W$$的线性映射。我们称$$ST$$为$$S$$和$$T$$的乘积。可以验证它具有乘积的大多数常见性质（交换性除外）：
  - 结合性：$$(T_{1}T_{2})T_{3}=T_{1}(T_{2}T_{3})$$
  - 恒等映射：$$TI=T,IT=T$$
  - 分配性质：$$(S_{1}+S_{2})T=S_{1}T+S_{2}T,S(T_{1}+T_{2})=ST_{1}+ST_{2}$$

## 零空间与值域

- 对于$$T\in\mathcal{L}(V,W)$$，$$V$$中被$$T$$映成$$\vec{0}$$的那些向量所组成的子集称为$$T$$的零空间（null space），记为$$\text{null} T$$
- 3.1命题：若$$T\in\mathcal{L}(V,W)$$，则$$\text{null}T$$是$$V$$的子空间，特别地$$\vec{0}\in\text{null}T$$。证明：用子空间的定义即得。于是，“零空间”这个名字中的“空间”就变得有意义了。
  - <a name="zero-space">注意</a>：<span style="color: blue">这个命题对于任意的线性映射都成立。而$$\text{null}T$$的元素虽然由$$T$$来确定，但是它作为一个子空间的存在并不依赖于$$T$$！</span>
- 线性映射称为单的，如果像相同蕴含原像相同。
- 3.2命题：设$$T\in\mathcal{L}(V,W)$$，则$$T$$是单的当且仅当$$\text{null}T=\{\vec{0}\}$$。由“单”的定义以及零空间的定义即得。
- 对于$$T\in\mathcal{L}(V,W)$$，由$$W$$中形如$$T\vec{v}(\vec{v}\in V)$$的向量组成的子集称为$$T$$的值域，记为$$\text{range}T=\{T\vec{v}:\vec{v}\in V\}$$。如果值域等于$$W$$则称该线性映射为满的。
  - 一个非常有用但比较抽象的结论是：对于算子$$T\in\mathcal{L}(V)$$，$$\text{range}(\text{range}T)\subseteq\text{range}T$$。换句话说，若$$\vec{v}\in\text{range}T$$，则$$T\vec{v}\in\text{range}T$$，即$$\text{range}T$$在$$T$$下是不变的
- 3.3命题：设$$T\in\mathcal{L}(V,W)$$，那么$$\text{range}T$$是$$W$$的子空间。由值域的定义以及子空间的定义即可证。
- 3.4命题：如果$$V$$是有限维向量空间，并且$$T\in\mathcal{L}(V,W)$$，那么$$\text{range}T$$是$$W$$的有限维子空间，并且$$\dim V=\dim\text{null}T+\dim\text{range}T$$。证明：用$$\text{null}T$$的一组基扩充成$$V$$的基，只需证明扩充出来的部分的像是$$\text{range}T$$的基。
- 3.5推论：如果$$V$$和$$W$$都是有限维向量空间，且$$\dim V>\dim W$$，那么从$$V$$到$$W$$的线性映射一定不是单的。由3.4和3.2可证。
- 3.6推论：如果$$V$$和$$W$$都是有限维向量空间，且$$\dim V<\dim W$$，那么从$$V$$到$$W$$的线性映射一定不是满的。由3.4和“满”的定义可证。
- 令$$T(x_{1},\cdots,x_{n})=\big(\sum_{k=1}^{n}a_{1,k}x_{k},\cdots,\sum_{k=1}^{n}a_{m,k}x_{k}\big)$$，利用3.5和3.6可证：
  - 当变量多于方程时，齐次线性方程组（即$$T\vec{x}=\vec{0}$$）必有非零解（易知$$\text{null}T$$严格大于$$\{\vec{0}\}$$，由3.5知这意味着$$n>m$$）。
  - 当方程多于变量时，必有一组常数项使得相应的非齐次线性方程组（即$$T\vec{x}=\vec{c}\ne\vec{0}$$）无解（无解意味着$$T$$不是满的，即存在某个$$\vec{c}$$，其不存在原像。由3.6知这意味着$$n< m$$）。

## 线性映射的矩阵

- 设$$T\in\mathcal{L}(V,W)$$，$$(\vec{v}_{1},\cdots,\vec{v}_{n})$$是$$V$$的基，$$\vec{w}_{1},\cdots,\vec{w}_{m}$$是$$W$$的基，那么对于每个$$k=1,\cdots,n$$，$$T\vec{v}_{k}$$都可以唯一地写成这些$$\vec{w}$$的线性组合：$$T\vec{v}_{k}=a_{1,k}\vec{w}_{1}+\cdots+a_{m,k}\vec{w}_{m}$$，其中$$a_{j,k}\in\mathbf{F},j=1,\cdots,m$$。因为线性映射由其在基上的值确定，所以线性映射$$T$$由这些标量$$a_{j,k}$$完全确定。由这些$$a$$构成的$$m\times n$$矩阵：

  $$
  \left[
  \begin{array}\\
    a_{1,1}&\cdots&a_{1,n}\\
    \vdots& &\vdots\\
    a_{m,1}&\cdots&a_{m,n}
  \end{array}
  \right]
  $$

  称为$$T$$关于基$$(\vec{v}_{1},\cdots,\vec{v}_{n})$$和基$$(\vec{w}_{1},\cdots,\vec{w}_{m})$$的矩阵，记为$$\mathcal{M}(T,(\vec{v}_{1},\cdots,\vec{v}_{n}),(\vec{w}_{1},\cdots,\vec{w}_{m}))$$，或简记为$$\mathcal{M}(T)$$。为方便记忆可以把矩阵写成这样：

  $$
  \begin{array}{cc}
    &
    \begin{matrix}
      \vec{v}_{1} \cdots & \vec{v}_{k} & \cdots \vec{v}_{n} \\
    \end{matrix} \\
    \begin{matrix}
      \vec{w}_{1} \\
      \vdots      \\
      \vec{w}_{m} \\
    \end{matrix} &
    \left[
    \begin{array} \\
      & & & a_{1,k} & & & \\
      & & & \vdots  & & & \\
      & & & a_{m,k} & & & \\
    \end{array}
    \right]
  \end{array}
  $$

  即第k列就是$$T\vec{v}_{k}$$用基$$(\vec{w}_{1},\cdots,\vec{w}_{m})$$表示时对应的系数。
- 两个矩阵的和定义为对应元素的和。这个定义恰好使得$$\mathcal{M}(T+S)=\mathcal{M}(T)+\mathcal{M}(S)$$成立。
- 矩阵的标量乘法定义为该标量乘以矩阵的每个元素。这个定义恰好使得$$\mathcal{M}(cT)=c\mathcal{M}(T)$$成立。
- 用上述两个定义可以产生一个向量空间：元素在$$\mathbf{F}$$中的所有$$m\times n$$矩阵之集记为$$\text{Mat}(m,n,\mathbf{F})$$。用向量空间的定义可证。
- 矩阵乘法的定义，恰好使得$$\mathcal{M}(TS)=\mathcal{M}(T)\mathcal{M}(S)$$成立
- $$\vec{v}$$的矩阵：设$$(\vec{v}_{1},\cdots,\vec{v}_{n})$$是$$V$$的基，如果$$\vec{v}\in V$$，那么存在唯一一组数$$b_{1},\cdots,b_{n}$$使得$$\vec{v}=b_{1}v_{1}+\cdots+b_{n}v_{n}$$。这样，定义$$\vec{v}$$的矩阵为：

  $$
  \mathcal{M}(\vec{v})=\left[\begin{array}\\b_{1}\\\vdots\\b_{n}\\\end{array}\right]
  $$
- 3.14命题：设$$T\in\mathcal{L}(V,W)$$，那么对于每个$$\vec{v}\in V$$都有$$\mathcal{M}(T\vec{v})=\mathcal{M}(T)\mathcal{M}(\vec{v})$$（注意这里的$$\mathcal{M}(T\vec{v})$$里的$$T\vec{v}$$是$$W$$上的元素）。按照定义展开即证得。
- 小结：到目前为止已涉及的向量空间的类型
  - $$\mathbf{F}^{n}$$，其元素是元组$$(x_{1},\cdots,x_{n})$$
  - $$\mathbf{F}^{n}$$到$$\mathbf{F}^{n}$$的线性映射的集合，其元素是单个线性映射（函数）
  - 线性映射的矩阵的集合$$\text{Mat}(m,n,\mathbf{F})$$，其元素是矩阵
    > 问：这个空间的元素和线性映射的空间的元素是一一对应的吗？目测是，因为在确定了$$V$$和$$W$$的一组基后，$$\mathcal{L}(V,W)$$中的每个变换就确定了一个矩阵，反之亦然。
      {: .lambda_question}

## 可逆性

- 线性映射$$T\in\mathcal{L}(V,W)$$称为可逆的，如果存在线性映射$$S\in\mathcal{L}(W,V)$$使得$$ST$$等于$$V$$上的恒等映射，**并且**$$TS$$等于$$W$$上的恒等映射。满足$$ST=I,TS=I$$的线性映射$$S\in\mathcal{L}(W,V)$$称为$$T$$的逆。
  > 问：是否存在$$S\in\mathcal{L}(W,V)$$使得$$ST$$等于$$V$$上的恒等映射，**但是**$$TS$$**不**等于$$W$$上的恒等映射？习题23解决了$$S\in\mathcal{L}(V)$$的情况，但是否对一般情况也适用？用下面的同构来证？
    {: .lambda_question}
- 3.17命题：一个线性映射是可逆的当且仅当它既是单的又是满的。证明：=>方向用定义即得。<=方向：对于每个$$\vec{w}\in W$$定义$$S\vec{w}$$是$$V$$中唯一使得$$T(S\vec{w})=\vec{w}$$的那个元素（根据$$T$$既单且满可得此元素的存在性和唯一性）。$$TS$$显然等于$$W$$上的恒等映射。要证明$$ST$$是$$V$$上的恒等映射，有$$T(ST\vec{v})=(TS)(T\vec{v})=I(T\vec{v})=T\vec{v}$$，这表明$$ST\vec{v}=\vec{v}$$（因$$T$$是单的）。除此以外还要证明$$S$$是一个线性映射（齐性+线性），用定义即可。
- 称两个向量空间是同构的，如果存在从一个到另一个的可逆线性映射。
- 3.18定理：两个有限维向量空间同构当且仅当它们的维数相等。证明：=>直接用3.4即得。<=用构造法：设$$(\vec{v}_{1},\cdots,\vec{v}_{n})$$和$$(\vec{w}_{1},\cdots,\vec{w}_{n})$$是该两个向量空间的基，定义$$T(a_{1}\vec{v}_{1}+\cdots+a_{n}\vec{v}_{n})=a_{1}\vec{w}_{1}+\cdots+a_{n}\vec{w}_{n}$$，可证明$$T$$既单且满从而该俩向量空间同构。
  - 这个定理表明，每个有限维向量空间都同构于某个$$\mathbf{F}^{n}$$
- 3.19命题：设$$(\vec{v}_{1},\cdots,\vec{v}_{n})$$和$$(\vec{w}_{1},\cdots,\vec{w}_{m})$$分别是$$V$$和$$W$$的基，那么$$\mathcal{M}$$是$$\mathcal{L}(V,W)$$和$$\text{Mat}(m,n,\mathbf{F})$$之间的可逆线性映射。证明：之前已经定义了矩阵的和以及标量乘法（因此$$\mathcal{M}$$是线性的），现在只需证明其既单且满即可，这很容易，略。
  - 注意，这里把$$\mathcal{M}$$看成一个函数，其定义域的元素是线性映射（也是函数）和对应的基的元组，值域的元素是矩阵。
  - 易知$$\text{Mat}(m,n,\mathbf{F})$$的维数是$$mn$$。
- 3.20命题：如果$$V$$和$$W$$都是有限维的，那么$$\mathcal{L}(V,W)$$是有限维的，并且$$\dim\mathcal{L}(V,W)=(\dim V)(\dim W)$$。由上述$$\dim\text{Mat}(m,n,\mathbf{F})=mn$$，3.18以及3.19即得。
- 一个向量空间到其自身的线性映射称为算子。用$$\mathcal{L}(V)=\mathcal{L}(V,V)$$表示$$V$$上算子的集合。
- 3.21定理：设$$V$$是有限维的，如果$$T\in\mathcal{L}(V)$$，那么下列等价：（a）$$T$$是可逆的（b）$$T$$是单的（c）$$T$$是满的
  - 证明：b=>c和c=>b都是用3.4式。

## 习题

- 习题22：设$$V$$是有限维的，并且$$S,T\in\mathcal{L}(V)$$，证明$$ST$$可逆当且仅当$$S$$和$$T$$都可逆
  - <=方向：设$$S$$的逆为$$S'$$，$$T$$的逆为$$T'$$，易证$$ST$$的逆为$$T'S'$$
  - =>方向：设$$ST$$的逆为$$K$$，则$$(ST)K=K(ST)=I$$，现在需证$$TK$$是$$S$$的逆。易证$$S(TK)=(ST)K=I$$，然后有两种方法可证$$(TK)S=I$$：
    - 第一种方法：由下面习题23直接得知！
    - 第二种方法：首先我们有$$TKS=TIKS=T(KST)KS=(TKS)(TKS)$$，令$$G=TKS$$，这里说明$$G=GG$$即$$G(G-I)$$是零映射。然后？
      > 然后怎么证？
        {: .lambda_question}
      
- 习题23：设$$V$$是有限维的，并且$$S,T\in\mathcal{L}(V)$$，证明$$ST=I$$当且仅当$$TS=I$$
  - 证明：先证$$T$$是单的。否则，根据3.2，设$$\vec{v}\ne\vec{0}$$且$$T\vec{v}=\vec{0}$$，由于$$(ST)\vec{v}=I\vec{v}=\vec{v}$$，但$$(ST)\vec{v}=S(T\vec{v})=S\vec{0}=\vec{0}$$，这与$$\vec{v}\ne\vec{0}$$矛盾。故T是单的。根据3.21，$$T$$是可逆的，设其逆为$$T'$$，则有$$T'T=TT'=I$$。现要证$$S=T'$$，有$$T'=IT'=(ST)T'=S(TT')=S$$。
- 习题24：设$$V$$是有限维的，并且$$T\in\mathcal{L}(V)$$，证明$$T$$是恒等映射的标量倍当且仅当对每个$$S\in\mathcal{L}(V)$$都有$$ST=TS$$。
  - =>方向：若$$T=aI$$，则$$ST=S(aI)=aS=aIS=TS$$
  - <=方向：一个非常不优美的方法是用矩阵$$\mathcal{M}(S)$$和$$\mathcal{M}(T)$$的乘积的元素的通项公式来证。有$$\mathcal{M}(S)$$的任意性可证得$$\mathcal{M}(T)$$是对角矩阵且对角线上元素相等。

# 第四章 多项式

## 次数

- 多项式：$$p(z)=a_{0}+a_{1}z+a_{2}z^{2}+\cdots+a_{m}z^{m},a_{m}\ne0$$，则$$m$$称为$$p$$的次数，记作$$\deg p$$。
- 4.1命题：设$$p\in\mathcal{P}(\mathbf{F})$$是$$m\ge1$$次多项式。令$$\lambda\in\mathbf{F}$$，则$$\lambda$$是$$p$$的根当且仅当存在$$m-1$$次多项式$$q\in\mathcal{P}\mathbf{F}$$使得$$p(z)=(z-\lambda)q(z),z\in\mathbf{F}$$。
  - 证明：<=是显然的。=>方向：由$$p(z)=p(z)-0=p(z)-p(\lambda)=(z-\lambda)(a_{1}+a_{2}q_{1}(z)+\cdots+a_{m}q_{m-1}(z))$$可得。
- 4.3推论：设$$p\in\mathcal{P}(\mathbf{F})$$是$$m\ge0$$次多项式，则$$p$$在$$\mathbf{F}$$中最多由$$m$$个互不相同的根。证明：由4.1和数学归纳法易得。
- 4.4推论：设$$a_{0},\cdots,a_{m}\in\mathbf{F}$$，如果$$a_{0}+a_{1}z+a_{2}z^{2}+\cdots+a_{m}z^{m}=0$$对$$z\in\mathbf{F}$$成立，则$$a_{0}=a_{1}=\cdots=a_{m}=0$$。证明：根据4.3，任何非负整数都不可能是这个多项式的次数（否则根据4.3其最多只有$$m$$个根，根这里条件说其有无数个根矛盾），于是所有系数都等于0。
- 4.5带余除法：设$$p,q\in\mathcal{P}(\mathbf{F})$$，并且$$p\ne0$$，则存在多项式$$s,r\in\mathcal{P}(\mathbf{F})$$，使得$$q=sp+r$$并且$$\deg r<\deg p$$。证明：取$$s\in\mathcal{P}(\mathbf{F})$$使得$$q-sp$$的次数最小，将其赋值为$$r$$，然后用反证法证明$$\deg r<\deg p$$。

## 复系数

- 4.7代数学基本定理：每个不是常数的复系数多项式都有根。证明需要用到Liouville定理，超出书本范围故略去。
  > 看看北大《高等代数》怎么证。
    {: .lambda_question}
  
- 4.8推论：如果$$p\in\mathcal{P}(\mathbf{C})$$是非常数多项式，则$$p$$可以唯一分解成这样的形式$$p(z)=c(z-\lambda_{1})\cdots(z-\lambda_{m})$$。用归纳法和4.7直接证得。

## 实系数

- 4.10命题：设$$p$$是实系数多项式，如果$$\lambda\in\mathbf{C}$$是$$p$$的根，则$$\overline{\lambda}$$也是$$p$$的根。证明：对$$a_{0}+a_{1}\lambda+\cdots+a_{m}\lambda^{m}=0$$两端取复共轭即得（要用到$$\overline{wz}=\overline{w}\text{ }\overline{z}$$）。
- 4.14定理：如果$$p\in\mathcal{P}(\mathbf{R})$$是非常数多项式，则$$p$$可以唯一（除因子的次序之外）分解成如下形式$$p(x)=c(x-\lambda_{1})\cdots(x-\lambda_{m})(x^{2}+\alpha_{1}x+\beta_{1})\cdots(x^{2}+\alpha_{M}x+\beta_{M})$$，其中$$c,\lambda_{1},\cdots,\lambda_{m}\in\mathbf{R},(\alpha_{1},\beta_{1}),\cdots,(\alpha_{M},\beta_{M})\in\mathbf{R}^{2}$$，并且对每个$$j$$都有$$\alpha_{j}^{2}<4\beta_{j}$$
  - 证明：就是用4.8和4.10。其中要注意的问题是，如果$$\lambda$$是一个非实数的复根，则根据4.10，$$\overline{\lambda}$$也是一个复根，但4.10并未说明这两个根的次数相同。解决办法是由$$p(x)=(x-\lambda)(x-\overline{\lambda})q(x)$$得$$q(x)=\frac{p(x)}{x^{2}-2(\text{Re }\lambda)x+\vert \lambda \vert^{2}}$$，可知对任何$$x\in\mathbf{R}$$都有$$q(x)\in\mathbf{R}$$，这样若把$$q$$写成$$q(x)=a_{0}+a_{1}x+\cdots+a_{n-1}x^{n-1}$$可看到$$0=\text{Im }q(x)=(\text{Im }a_{0})+(\text{Im }a_{1})x+\cdots+(\text{Im }a_{n-2})x^{n-2}$$，然后由4.4证明这些虚部都是0。最后，唯一性用4.8来证。

# 第五章 本征值与本征向量

本章主要研究算子（到自身的线性映射）。

## 不变子空间

- 对于$$T\in\mathcal{L}(V)$$和$$V$$的子空间$$U$$，如果对每个$$\vec{u}\in U$$都有$$T\vec{u}\in U$$，则称$$U$$在$$T$$下是不变的。不变子空间的一些例子：
  - $\{\vec{0}\}$
  - $\text{null}T$
  - $\text{range}T$
- 对于$$T\in\mathcal{L}(V)$$和标量$$\lambda\in\mathbf{F}$$，如果有非零向量$$\vec{u}\in V$$使得$$T\vec{u}=\lambda\vec{u}$$，则称$$\lambda$$为$$T$$的本征值，而$$\vec{u}$$是$$T$$的（相应于$$\lambda$$的）本征向量。
  - 注意，$$\vec{u}$$必须是非零，但$\lambda$可以是0
  - $$T\vec{u}=\lambda\vec{u}$$等价于$$(T-\lambda I)\vec{u}=\vec{0}$$，因此：
    - $$\lambda$$是$$T$$的本征值当且仅当$$T-\lambda I$$不是单的
    - 对有限维向量空间而言，根据3.21，$$\lambda$$是$$T$$的本征值当且仅当$$T-\lambda I$$不可逆，当且仅当$$T-\lambda I$$不是满的
    - $$T$$的相应于$$\lambda$$的本征向量之集等于$$\text{null}(T-\lambda I)$$，而且是$$V$$的子空间（看[这里](#zero-space)）
  - 注意：同一个本征值可以有多于一个不同的且线性无关的本征向量。如$$I$$的本征值为1，但$$\mathbf{R}^{2}$$上的$$I$$就有两个不同的本征向量$$(0,1)$$和$$(1,0)$$。
  - 例子：对于算子$$T\in\mathcal{L}(\mathbf{F}^{2}),T(\vec{w},\vec{z})=(-\vec{z},\vec{w})$$。若$$\mathbf{F}=\mathbf{R}$$，此算子有很好的几何解释（绕$$\mathbf{R}^{2}$$的原点逆时针转$$90^{\circ}$$，易知$$T$$没有本征值。若$$\mathbf{F}=\mathbf{C}$$，$$i$$和$$-i$$都是$$T$$的本征值。
- 5.6定理：设$$T\in\mathcal{L}(V)$$，$$\lambda_{1},\cdots,\lambda_{m}$$是$$T$$的互不相同的本征值，$$\vec{v}_{1},\cdots,\vec{v}_{m}$$是相应的非零本征向量，则$$(\vec{v}_{1},\cdots,\vec{v}_{m})$$线性无关。
  - 证明：反证法，设$$k$$是使$$\vec{v}_{k}\in\vspanr{v}{k-1}$$成立的最小正整数（由2.4知道这样的$$k$$一定存在），于是有$$\vec{v}_{k}=a_{1}\vec{v}_{1}+\cdots+a_{k-1}\vec{v}_{k-1}$$，然后用该式两端乘以$$\lambda_{k}$$得到的式子减去把$$T$$作用于该等式两端得到的式子，根据$$(\vec{v}_{1},\cdots,\vec{v}_{k-1})$$的线性无关性易知这些$$a$$都是0，从而$$\vec{v}_{k}$$等于$$\vec{0}$$，与特征向量不为零的假设矛盾。
- 5.9推论：$$V$$上的每个算子最多有$$\dim V$$个互不相同的本征值。证明：由5.6即得。

## 多项式对算子的作用

- 定义：算子的幂，$$T^{0}$$定义为恒等算子
- 定义：$$p(T)=a_{0}I+a_{1}T+a_{2}T^{2}+\cdots+a_{m}T^{m}$$
  - 容易验证：对于一个固定的算子$$T\in\mathcal{L}(V)$$，由$$p\mapsto p(T)$$所给出的从$$\mathcal{P}(\mathbf{F})$$到$$\mathcal{L}(V)$$的函数是线性的。
- 乘法性质：易证$$p(T)q(T)=q(T)p(T)$$（直接展开然后用$$T$$的交换律即可）

## 上三角矩阵

- 5.10定理：有限维非零复向量空间上的每个算子都有本征值。
  - 证明：设$$V$$是$$n>0$$维复向量空间，$$T\in\mathcal{L}(V)$$，取$$\vec{v}\in V$$使得$$\vec{v}\ne\vec{0}$$。因为$$V$$是$$n$$维的，所以$$n+1$$个向量$$(\vec{v},T\vec{v},T^{2}\vec{v},\cdots,T^{n}\vec{v})$$必定线性相关，因此有不全为零的复数$$a_{0},\cdots,a_{n}$$使得$$\mathbf{0}=a_{0}\vec{v}+a_{1}T\vec{v}+\cdots+a_{n}T^{n}\vec{v}$$。对其作多项式分解（4.8）得$$\mathbf{0}=c(T-\lambda_{1}I)\cdots(T-\lambda_{m}I)\vec{v}$$对于某个$$c$$和某个$$0\lt m\le n$$成立，而这意味着存在某个$$j$$使得$$(T-\lambda_{j+1}I)\cdots(T-\lambda_{m}I)\vec{v}\ne\vec{0}$$但$$(T-\lambda_{j}I)\cdots(T-\lambda_{m}I)\vec{v}=\vec{0}$$，因此$$T-\lambda_{j}I$$不是单的，即$$T$$有本征值。
- 一个矩阵称为上三角的，如果位于对角线下方的元素全为0。注意：对角线上的元素是否为0并没有限制。
- 5.12命题：设$$T\in\mathcal{L}(V)$$，并且$$(\vec{v}_{1},\cdots,\vec{v}_{n})$$是$$V$$的基，则下列等价：
  - (a)$$T$$关于基$$(\vec{v}_{1},\cdots,\vec{v}_{n})$$的矩阵是上三角的
  - (b)$$T\vec{v}_{k}\in\vspanr{v}{k},k=1,\cdots,n$$
  - (c)$$\vspanr{v}{k}$$在$$T$$下是不变的，$$k=1,\cdots,n$$

  证明：显然。
- 5.13定理：设$$V$$是复向量空间，并设$$T\in\mathcal{L}(V)$$，则$$T$$关于$$V$$的某个基具有上三角矩阵。
  - 证明：对$$V$$的维数用归纳法。若$$\dim V=1$$结论显然成立。设$$\dim V>1$$，并设对于**所有**维数比$$V$$小的复向量空间结果都成立。设$$\lambda$$是$$T$$的任意本征值（5.10），设$$U=\text{range}(T-\lambda I)$$，由3.21知$$T-\lambda I$$不是满的，故$$\dim U<\dim V$$。通过$$T\vec{u}=(T-\lambda I)\vec{u}+\lambda\vec{u}$$易证$$U$$在$$T$$下是不变的，因此$$T\vert_{U}$$是$$U$$上的算子。由归纳法假设，$$U$$有基$$(\vec{u}_{1},\cdots,\vec{u}_{m})$$使得$$T\vert_{U}$$关于此基有上三角矩阵，因此根据5.12对每个$$j$$都有$$T\vec{u}_{j}=(T\vert_{U})(\vec{u}_{j})\in\vspanr{u}{j}$$。把$$(\vec{u}_{1},\cdots,\vec{u}_{m})$$扩充成$$V$$的基$$(\vec{u}_{1},\cdots,\vec{u}_{m},\vec{v}_{1},\cdots,\vec{v}_{n})$$，则对每个$$k$$都有$$T\vec{v}_{k}=(T-\lambda I)\vec{v}_{k}+\lambda\vec{v}_{k}$$，其中第一项$$\in U=\vspanr{u}{m}$$，因此$$T\vec{v}_{k}=\vspan{\vec{u}_{1},\cdots,\vec{u}_{m},\vec{v}_{1},\cdots,\vec{v}_{n}}$$，而由5.12知$$T$$关于基$$(\vec{u}_{1},\cdots,\vec{u}_{m},\vec{v}_{1},\cdots,\vec{v}_{n})$$有上三角矩阵。
- 5.16命题：设$$T\in\mathcal{L}(V)$$关于$$V$$的某个基有上三角矩阵，则$$T$$可逆当且仅当这个上三角矩阵对角线上的元素都不是0。
  - 证明：只需证明等价命题：$$T$$不可逆当且仅当某个元素是0。设$$(\vec{v}_{1},\cdots,\vec{v}_{n})$$是$$V$$的基使$$T$$关于此基有上三角矩阵。

    $$
    \left[
    \begin{array}\\
      \lambda_{1} & & & *\\
      & \lambda_{2} & & \\
      & & \ddots & \\
      0 & & & \lambda_{n}
    \end{array}
    \right]
    $$

    - <=方向：若$$\lambda_{1}=0$$则$$T\vec{v}_{1}=\vec{0}$$，故$$T$$不可逆。否则设$$\lambda_{k}=0,1\lt k\le n$$，由5.17，$$T\vec{v}_{k}\in\vspanr{v}{k-1}$$，根据3.5，存在非零向量$$\vec{v}\in\vspanr{v}{k}$$使得$$T\vec{v}=0$$，故$$T$$不可逆。
    - =>方向：假设$$T$$不可逆，因此它不是单的且有非零向量$$\vec{v}\in V$$使得$$\vec{0}=T\vec{v}=T(a_{1}\vec{v}_{1}+\cdots+a_{k}\vec{v}_{k})=(a_{1}T\vec{v}_{1}+\cdots+a_{k-1}T\vec{v}_{k-1})+a_{k}T\vec{v}_{k};a_{1},\cdots,a_{k}\in\mathbf{F};a_{k}\ne0$$（把$$\vec{v}$$表示成基的线性组合然后取$$k$$是系数不为0的最大下标）。最后那个括号中的向量含于$$\vspanr{v}{k-1}$$（因为$$T$$关于该基的矩阵是上三角的，5.12），于是$$a_{k}T\vec{v}_{k}$$从而$$T\vec{v}_{k}$$也含于$$\vspanr{v}{k-1}$$，于是若把$$T\vec{v}_{k}$$写成基$$(\vec{v}_{1},\cdots,\vec{v}_{n})$$的线性组合，则$$\vec{v}_{k}$$的系数即$$\lambda_{k}$$是0。
- 5.18命题：设$$T\in\mathcal{L}(V)$$关于$$V$$的某个基有上三角矩阵，则这个上三角矩阵对角线上的元素恰好是$$T$$的所有本征值。
  - 证明：设$$T$$关于某个基有上三角矩阵$$\mathcal{M}(T)$$，设$$\lambda\in\mathbf{F}$$，则矩阵$$\mathcal{M}(T-\lambda I)$$也是一个上三角矩阵且对角线上元素为$$\lambda_{k}-\lambda$$。根据5.16，$$T-\lambda I$$不可逆（即其不是单的，即存在非零向量被其映成零向量）当且仅当$$\lambda$$等于某个$$\lambda_{j}$$，即$$\lambda$$是$$T$$的本征值当且仅当$$\lambda$$等于某个$$\lambda_{j}$$。

## 对角矩阵

- 易知$$T\in\mathcal{L}(V)$$关于$$V$$的某个基有对角矩阵当且仅当$$V$$有一个由$$T$$的本征向量组成的基
- 5.20命题：若$$T\in\mathcal{L}(V)$$由$$\dim V$$个互不相同的本征值，则$$T$$关于$$V$$的某个基有对角矩阵
  - 注意逆命题不成立，某些本征值较少的算子也可能有对角矩阵
  - 证明：设$$T\in\mathcal{L}(V)$$有$$\dim V$$个互不相同的本征值$$\lambda_{1},\cdots,\lambda_{\dim V}$$，根据5.6，它们对应的非零本征向量线性无关，根据2.17，这些本征向量是$$V$$的基，因此$$T$$关于由这些本征向量组成的基由对角矩阵。
- 5.21命题：设$$T\in\mathcal{L}(V)$$，并设$$\lambda_{1},\cdots,\lambda_{m}$$是$$T$$的所有互不相同的本征值，则下列等价：
  - (a)$$T$$关于$$V$$的某个基有对角矩阵
  - (b)$$V$$有一个由$$T$$的本征向量组成的基
  - (c)$$V$$有在$$T$$下不变的1维子空间$$U_{1},\cdots,U_{n}$$使得$$V=U_{1}\oplus\cdots\oplus U_{n}$$
  - (d)$$V=\text{null}(T-\lambda_{1}I)\oplus\cdots\oplus\text{null}(T-\lambda_{m}I)$$
  - (e)$$\dim V=\dim\text{null}(T-\lambda_{1}I)+\cdots+\dim\text{null}(T-\lambda_{m}I)$$

  证明：(a)和(b)等价已在之前说明。(b)=>(c)可令$$U_{j}=\vspan{\vec{v}_{j}}$$，其中$$\vec{v}_{j}$$是$$T$$的其中一个本征向量。(c)=>(b)对每个$$\vec{v}_{j}\in U_{j}$$，它都是$$T$$的本征向量，又由直和的条件易知$$(\vec{v}_{1},\cdots,\vec{v}_{n})$$是$$V$$的基，即得。(b)=>(d)易知$$V$$中每个向量都是$$T$$的本征向量的线性组合，故$$V=\text{null}(T-\lambda_{1}I)+\cdots+\text{null}(T-\lambda_{m}I)$$，现只需证明这个和是直和；对于任意$$\vec{u}_{j}\in\text{null}(T-\lambda_{j}I)$$，要使$$\vec{0}=\vec{u}_{1}+\cdots+\vec{u}_{m}$$成立，它们只能全都等于0（根据5.6），从而前述的和是直和。(d)=>(e)由第二章习题17即得。(e)=>(b)在每个$$\text{null}(T-\lambda_{j}I)$$中取一个基，全部这些基组成$$T$$的一组本征向量$$(\vec{v}_{1},\cdots,\vec{v}_{n})$$，由(e)的条件知道$$n=\dim V$$，现要证它们线性无关，即$$a_{1}\vec{v}_{1}+\cdots+a_{n}\vec{v}_{n}=\vec{0}$$的所有$$a$$都是$$0$$，只需对它们分组然后用5.6知每组都是$$\vec{0}$$，而每组内的$$\vec{v}_{k}$$是$$\text{null}(T-\lambda_{j}I)$$的基从而所有$$a_{k}$$都是0，即得。

## 实向量空间的不变子空间

- 5.24定理：在有限维非零实向量空间中，每个算子都有1维或2维的不变子空间。
  - 证明：设$$V$$是$$n>0$$维实向量空间，$$T\in\mathcal{L}(V)$$，取$$\vec{v}\in V,\vec{v}\ne0$$，由$$n+1$$个向量的线性相关性知存在不全为零的实数$$a_{0},\cdots,a_{n}$$使得$$\vec{0}=a_{0}\vec{v}+a_{1}T\vec{v}+\cdots+a_{n}T^{n}\vec{v}$$，根据4.14可以作分解$$\vec{0}=c(T-\lambda_{1}I)\cdots(T-\lambda_{m}I)(T^{2}+\alpha_{1}T+\beta_{1}I)\cdots(T^{2}+\alpha_{M}T+\beta_{M}I)\vec{v}$$，这意味着至少有一个$$j$$使得$$T-\lambda_{j}I$$不是单的或者$$(T^{2}+\alpha_{j}T+\beta_{j}I)$$不是单的。若是前者，则$$T$$有1维不变子空间。若是后者，则存在$$\vec{u}\in V, \vec{u}\ne\vec{0}$$，我们考虑$$\vspan{\vec{u},T\vec{u}}$$的一个典型元素$$a\vec{u}+bT\vec{u};a,b\in\mathbf{R}$$，对其应用$$T$$并且条件$$T^{2}\vec{u}+\alpha_{j}T\vec{u}+\beta_{j}\vec{u}=\vec{0}$$知$$T(a\vec{u}+bT\vec{u})\in\vspan{\vec{u},T\vec{u}}$$，于是$$\vspan{\vec{u},T\vec{u}}$$在$$T$$下不变。
- 设$$V=U\oplus W$$，则每个$$\vec{v}\in V$$都可表示为$$\vec{v}=\vec{u}+\vec{w};\vec{u}\in U,\vec{w}\in W$$。定义线性映射$$P_{U,W}\in\mathcal{L}(V)$$为：$$P_{U,W}\vec{v}=\vec{u}$$。这和“投影”类似。性质包括：
  - 对每个$$\vec{v}\in V$$都有$$\vec{v}=P_{U,W}\vec{v}+P_{W,U}\vec{v}$$
  - 多次应用，结果不变：$$P^{2}_{U,W}=P_{U,W}$$
  - $\text{range}P_{U,W}=U$
  - $\text{null}P_{U,W}=W$
- 5.26定理：在奇数维实向量空间上，每个算子都有本征值。
  - 证明：对维数用归纳法。若维数为1，显然成立。设$$\dim V>1$$是奇数，且结论对$$\dim V-2$$成立。设$$T\in\mathcal{L}(V)$$，若$$T$$由本征值则证明结束，否则由5.24知$$V$$有在$$T$$下不变的2维子空间$$U$$，由2.13知存在$$V$$的子空间$$W$$使得$$V=U\oplus W$$。注意不能直接对$$W$$和$$T\vert W$$用归纳法因为$$W$$可能不是在$$T$$下不变的。定义$$S\in\mathcal{L}(W)$$为$$S\vec{w}=P_{W,U}(T\vec{w}),\vec{w}\in W$$。由归纳法假设$$S$$有一个本征值$$\lambda$$，其对应的非零本征向量为$$\vec{w}\in W$$。如果$$\vec{w}$$是$$T$$的相应于本征值$$\lambda$$的本征向量，则证明结束。否则考虑$$U+\vspan{\vec{w}}$$中的一个典型向量$$\vec{u}+a\vec{w}$$，易得$$(T-\lambda I)(\vec{u}+a\vec{w})=T\vec{u}-\lambda\vec{u}+aP_{U,W}(T\vec{w})\in U$$，因此$$T-\lambda I$$把$$U+\vspan{\vec{w}}$$映到$$U$$内。由于$$U+\vspan{\vec{w}}$$的维数比$$U$$的维数大，故$$(T-\lambda I)\vert_{U+\vspan{\vec{w}}}$$不是单的（3.5），于是$$T$$有本征值。
