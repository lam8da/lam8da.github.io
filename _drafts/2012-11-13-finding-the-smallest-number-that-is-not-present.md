---
layout: post
title: 找出一个无序正整数数组中未出现的最小正整数
categories:
    - 算法&数学
tags:
    - 算法
    - 排序
    - Work in Progress
---

{::comment} Not show line_numbers {:/comment}
{::options syntax_highlighter_opts="{default_lang: c++ \}" /}

* TOC
{:toc}

有一个长度为n的无序正整数数组，找出其中中未出现的最小正整数，要求时间复杂度O(n)，空间复杂度O(1)。

直接使用桶排序即可。
