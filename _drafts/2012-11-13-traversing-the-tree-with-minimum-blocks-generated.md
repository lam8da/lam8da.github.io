---
layout: post
title: 如何遍历一棵树使得块数最少
categories:
    - 算法&数学
tags:
    - Work in Progress
    - 树
    - 算法
---

{::comment} Not show line_numbers {:/comment}
{::options syntax_highlighter_opts="{default_lang: c++ \}" /}

* TOC
{:toc}

来自网易游戏的面试题：一棵多叉树，每个点都有一个标识，不同的点的标识可以相同。现在把树遍历，遍历的规则是对于任意一棵子树一定要先把根遍历了才能遍历其他结点。然后按照点的遍历顺序生成一个标识序列，序列中相邻的相同标识组成一个块，目标是使得序列的块数最少。
