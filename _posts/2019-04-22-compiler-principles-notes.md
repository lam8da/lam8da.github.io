---
layout: post
title: 编译原理笔记
categories:
    - 编译原理
tags:
    - Work in Progress
    - 编译原理
---

{::comment} Not show line_numbers {:/comment}
{::options syntax_highlighter_opts="{default_lang: c++ \}" /}

* TOC
{:toc}

本文主要是编译原理书籍的一些笔记。内容主要来自以下书籍：

- 《编译器设计/Engineering a Compiler》

# 词法分析器

## RE，NFA，DFA的关系

- RE到NFA：Thompson构造法。就是每种正则表达式的基本构造（$$ab$$，$$a\vert b$$，$$a\ast b$$）都有对应的NFA的模板。
- NFA到DFA：子集构造法。就是从集合$$\{n_0\}$$开始，将其$$\epsilon{-}closure$$作为一个所求DFA的状态；然后把每个这样得到的状态在经过$$\Sigma$$上每个字符转移后得到的集合的$$\epsilon{-}closure$$也作为新的状态；直到没有新的状态为止。
- 最小化DFA：Hopcroft算法。一开始把初始结点作为一个集合，除初始结点外的其他结点作为另一个集合。每次从已有集合选出一个，判断是否有$$\Sigma$$上的字符可以将该集合分成两组，使得该两组集合经过该字符转以后到达不同的已有集合。如果是，就把该集合替换为该两个新集合。直到没有更多的拆分为止。最后每个集合作为所求DFA的一个状态。
  > 如何证明正确性？
    {: .lambda_question}
- 最小化DFA的另一种算法：直接从NFA构造最小DFA的Brzozowski算法。观察得知：子集构造法总是生成不包含重复前缀路径的DFA
  > 如何证明？
    {: .lambda_question}
  根据这种观察，我们令：
  - $$reverse(n)$$是NFA
    $$n$$经过这些操作得到的NFA：翻转$$n$$中所有转移的方向，将初始状态设为$$reverse(n)$$的接受状态，增加一个新的初始状态，将其通过$$\epsilon$$边连接到$$n$$中所有的接受状态
  - $$reachable(n)$$返回NFA
    $$n$$中从初始状态可到达的状态和转移的集合（即删掉不可达状态和转移后的子NFA）
  - $$subset(n)$$为对$$n$$应用子集构造法生成的DFA
  - 这样，给定NFA
    $$n$$，等价的最小DFA就是下述表达式：$$reachable(subset(reverse(reachable(subset(reverse(n))))))$$
    > 如何证明？而且有何实际意义？
      {: .lambda_question}
- DFA到RE：Kleen构造法。基本思想：DFA是一个图，如果图无环，构造算法是显然的：枚举所有路径即可（路径数量有限）；如果有环，可能的环的种类也是有限的，通过RE的闭包运算可以模拟这些环。
  <img src="{{ site.url }}/assets/2019-04-22-compiler-principles-notes/2.18.png" alt="2.18" style="margin: 4px">
  - 上述算法中，$$R_{ij}^k$$描述了DFA中从状态$$i$$到状态$$j$$、不经由编号大于$$k$$的状态的所有路径。这里“经由”表示“进入且离开”。

## 词法分析器

- 表驱动的词法分析器
  - 识别token的基本算法
    <img src="{{ site.url }}/assets/2019-04-22-compiler-principles-notes/2.14.png" alt="2.14" style="margin: 4px">
  - 上述算法会导致平方级别的回滚调用。考虑正则表达式$$ab\vert(ab)\ast
    c$$，对于串$$abababab$$会导致平方级别回滚，因为在读到终结符前都不会到达$$s_e$$。下面改进的算法通过保存额外的失败信息可以避免这个问题。基本思想就是找出上面算法中在重复过去相同的回滚时所做的重复计算，然后把这些重复计算的结果在第一次计算时就保存下来：
    <img src="{{ site.url }}/assets/2019-04-22-compiler-principles-notes/2.15.png" alt="2.15" style="margin: 4px">
- 直接编码的词法分析器：降低了计算DFA转移的成本，将原本显式表示的DFA状态和转移图替换为隐式表示方法。即：把转移实现为函数调用，对每个不同的状态都有一个单独的$$NextChar()$$函数与之对应，每个函数内都编码了如$$lexeme\gets lexeme+char$$的操作，因此代码有很多重复
- 手工编码的词法分析器：对直接编码的词法分析器高度优化以减少分析器和系统其余组件之间的接口的开销
    
# 语法分析器

- 各个语言的包含关系：$$RG(Regular Grammar)\subset LL(1)\subset LR(1)\subset
  CFG(Context Free Grammar)$$
  - 任意CFG需要花费更多的时间进行语法分析。如Earley算法可以在$$O(n^3)$$时间内解析任意CFG
  - LR(1)语法包含了无歧义CFG的很大一个子集，可以在线性时间内扫描输入并自底向上进行语法分析，任何时候都只需从当前输入符号前瞻最多一个单词
  - LL(1)可以在线性时间内扫描输入并自顶向下（通过手工编码的递归下降分析器或生成的LL(1)分析器）进行语法分析，只需前瞻一个单词
- 自顶向下的语法分析
  - 自顶向下的语法分析可以高效进行的一个关键点是：上下文无关语法的很大一个子集不进行回溯即可以完成语法分析

## 自顶向下语法分析

- 自顶向下语法分析器可能会无限循环（因为左递归）。很容易消除直接左递归（略），消除间接左递归的算法如下（将间接左递归转换为直接左递归，再重写直接左递归为右递归）：
  <img src="{{ site.url }}/assets/2019-04-22-compiler-principles-notes/3.6.png" alt="3.6" style="margin: 4px">
- 解决了无限循环问题后还是有可能会回溯。可以利用一个简单修改来（尝试）避免回溯：在选择下一条产生式时，可以同时考虑当前关注的符号以及下一个输入符号（前瞻符号）。如果这样做后不需要回溯，就说该语法在前瞻一个符号时是无回溯的。形式化定义如下：
  - $$FIRST(\alpha)$$定义为语法符号$$\alpha$$推导出的语句开头可能出现的终结符的集合。$$\epsilon$$和$$eof$$同时出现在$$FIRST$$的定义域和值域中。可以通过一个简单的不动点算法算出每个语法符号的$$FIRST$$集合

# 参考

- [Lattics: Too Much Theory](http://cliffc.org/blog/2012/02/12/too-much-theory/)
- [Lattics: Too Much Theory Part 2](http://cliffc.org/blog/2012/02/27/too-much-theory-part-2/)
- [Lattics: Too Much Theory Part 3](http://cliffc.org/blog/2012/03/24/too-much-theory-part-3/)
