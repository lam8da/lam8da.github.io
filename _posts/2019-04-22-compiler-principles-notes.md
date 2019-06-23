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

- RE到NFA：Thompson构造法。就是每种正则表达式的基本构造（$$ab$$，$$a\vert b$$，$$a\ast b$$）都有对应的NFA的模板。
- NFA到DFA：子集构造法。就是从集合$$\{n_0\}$$开始，将其$$\epsilon{-}closure$$作为一个所求DFA的状态；然后把每个这样得到的状态在经过$$\Sigma$$上每个字符转移后得到的集合的$$\epsilon{-}closure$$也作为新的状态；直到没有新的状态为止。
- 最小化DFA：Hopcroft算法。一开始把初始结点作为一个集合，除初始结点外的其他结点作为另一个集合。每次从已有集合选出一个，判断是否有$$\Sigma$$上的字符可以将该集合分成两组，使得该两组集合经过该字符转以后到达不同的已有集合。如果是，就把该集合替换为该两个新集合。直到没有更多的拆分为止。最后每个集合作为所求DFA的一个状态。
  > 如何证明正确性？
    {: .lambda_question}
- RE实现词法分析器
  - 基本算法
    <img src="{{ site.url }}/assets/2019-04-22-compiler-principles-notes/2.14.png" alt="2.14" style="margin: 4px">

# 参考

- [Lattics: Too Much Theory](http://cliffc.org/blog/2012/02/12/too-much-theory/)
- [Lattics: Too Much Theory Part 2](http://cliffc.org/blog/2012/02/27/too-much-theory-part-2/)
- [Lattics: Too Much Theory Part 3](http://cliffc.org/blog/2012/03/24/too-much-theory-part-3/)
