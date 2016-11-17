---
layout: post
title: 
categories:
    - 随机在一个圆内生成n个满足均匀分布的点
tags:
    - Work in Progress
    - 
---

{::comment} Not show line_numbers {:/comment}
{::options syntax_highlighter_opts="{default_lang: c++ \}" /}

* TOC
{:toc}

0. 直接在一个包含该圆的正方形上随机生成一个点，若落到圆内就生成下一个，否则丢弃。

1. 算出圆的面积为S，随机生成一个[0,S]的数然后映射为一个点。这样貌似不行？因为一条边上有无限个点，而边的面积为0。

2. 随机选一个极角，然后随机（按照某种概率分布）选择一个半径？
