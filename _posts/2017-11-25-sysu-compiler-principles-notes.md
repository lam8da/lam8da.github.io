---
layout: post
title: 中大编译原理课程笔记
categories:
    - 编译原理
tags:
    - Work in Progress
    - 课程笔记
    - 编译原理
---

{::comment} Not show line_numbers {:/comment}
{::options syntax_highlighter_opts="{default_lang: c++ \}" /}

* TOC
{:toc}

## 笔记

1. 编译器一般结构：
   - 词法分析（Lexical Analysis or Scanning）：将字符序列转化为单词（token）序列，给出单词的类别和一些相关属性值属性值放在公用的符号表（Symbol Table）中，词法分析过程返回单词的属性值存放的地址
   - 语法分析（Syntax Analysis）：在分词的基础上建立语法分析树
   - 语义分析（Semantic Analysis）：类型检查和类型转换
   - 中间代码生成（Intermediate Code Generation）
   - 代码优化（Code Optimization）：优化中间代码
   - 代码生成（Code Generation）
1. 词法分析：
   - 正则语言（用通常的正则表达式定义的语言）如何识别
     - 非确定有限自动机（NFA）。构建方法是，为每种类型的正则表达式单元（例如或、星号等）构建结点或结点组，并构建起始结点和终结点，然后将它们通过$$\epsilon$$符号连起来。如何用来识别语言：通过$$\epsilon\text{-}closure$$实现，从初始结点的$$\epsilon\text{-}closure$$开始，每读入一个字符就计算转移到的结点集合的$$\epsilon\text{-}closure$$，重复这个过程直到读入所有字符，如果终结点在最后得到的$$\epsilon\text{-}closure$$中就判断成功否则失败。
     - 确定有限自动机（DFA）。构建方法是：
       - 从NFA到DFA：类似直接用NFA识别语音的过程，把$$\epsilon\text{-}closure(s_0)$$作为一个确定状态（其中$$s_0$$是初始的NFA状态），对每个状态和每个符号，找到该状态通过该转移符号可达的所有结点，并算出其$$\epsilon\text{-}closure$$作为新的状态。重复此过程直到没有新的状态为止。缺点是这个转换可能导致状态空间的指数增长，例如与$$(a\vert b)^*a(a\vert b)^{n-1}$$的NFA等价的状态书不少于$$2^n$$。
     - 工具：[lex](http://dinosaur.compilertools.net/lex/)、[flex](https://sourceforge.net/projects/flex/)可以用来生成一个scanner，用来识别指定类型的token。例如可以通过正则表达式定义需要识别的token形式以及相应的动作（怎样处理，例如print出来），将其输入flex可以生成一个c源程序，编译之得到一个c可执行程序，可用来处理包含需要识别的token的文件并在每次识别成功时都进行相应的处理。
1. 语法分析：
   - 工具：[YACC](http://dinosaur.compilertools.net/yacc/)、[Bison](http://dinosaur.compilertools.net/bison/)可以用来生成语法分析程序。用法是通过[BNF语法](https://zh.wikipedia.org/wiki/%E5%B7%B4%E7%A7%91%E6%96%AF%E8%8C%83%E5%BC%8F)定义出一个上下无关文法的语法结构，然后用c定义出各个AST（抽象语法树）的节点类型，然后在该语法结构中指定如果解析到对应的语法结构要怎样做（例如，怎样用c创建一个AST的结点）。执行时返回一棵语法树，通过我们自定义的AST节点类型表示。详见[这里](https://lesliezhu.github.io/2014/05/29/构建Toy编译器——基于Flex-Bison和LLVM)。

Next: Lecture04.pdf

## 参考资料

- 自动机理论：<https://zhuanlan.zhihu.com/pikanote>

