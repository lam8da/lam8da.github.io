---
layout: post
title: 《算法谜题》读书笔记
categories:
    - 
tags:
    - Work in Progress
    - 
---

{::comment} Not show line_numbers {:/comment}
{::options syntax_highlighter_opts="{default_lang: c++ \}" /}

* TOC
{:toc}

1. 为什么3阶幻方中间的数字一定是5？
   - 解：
1. 给定一个m*n的实数表格，能否找出一个算法，能够在仅允许将一整行或一整列的数字改变符号的前提下，使得所有的行和列之和非负？
   - 解：能。用归纳法可以证明：假设对于m*(n-1)的表格存在这样的算法，对于m*n表格，先把其由m行以及前n-1列构成的子表格通过该算法运行一次使得该m*(n-1)表格满足条件。
