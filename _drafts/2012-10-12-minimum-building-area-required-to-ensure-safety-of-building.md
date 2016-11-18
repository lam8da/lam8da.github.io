---
layout: post
title: 保证楼房安全的最短占地面积
categories:
    - 算法&数学
tags:
    - Work in Progress
    - 贪心
    - 面试题
    - 算法
---

{::comment} Not show line_numbers {:/comment}
{::options syntax_highlighter_opts="{default_lang: c++ \}" /}

1. TOC
{:toc}

原题两种方法，一种是贪心一种是递归。

扩展：有\\(n\\)栋楼排成一排，要求任意两栋楼同时倒下时都保证不碰到对方，即任意（不一定相邻）两栋楼间的距离至少是这两栋楼的高度和，求最小占地长度
