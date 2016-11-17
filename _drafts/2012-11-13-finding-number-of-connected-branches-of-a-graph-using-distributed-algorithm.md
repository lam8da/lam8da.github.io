---
layout: post
title: 
categories:
    - 如何使用分布式找出一个图的连通分支数
tags:
    - Work in Progress
    - 
---

{::comment} Not show line_numbers {:/comment}
{::options syntax_highlighter_opts="{default_lang: c++ \}" /}

* TOC
{:toc}

一些想法：

1. 使用并查集的思想，每个点维护两个值：其邻接点列表、其当前表示（以某个点的id标识，就像并查集那样）
2. 使用Pregel的最坏复杂度：图的直径？例如若该图是一条链就会悲剧
3. 优化：每次找到图的一个匹配，把匹配的每条边的两个点缩成一个，然后继续找匹配继续缩。每次点数减少了一半？可以在log(点数)的时间内找出答案？
4. 3的成立涉及到任意图极大匹配的下界问题

 
