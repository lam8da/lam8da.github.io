---
layout: post
title: 《数学之美》笔记
categories:
    - 人工智能
tags:
    - 数学
    - 机器学习
    - 读书笔记
---

* TOC
{:toc}

## 第3章 统计语言模型

1. 在自然语言中，上下文之间的相关性可能跨度非常大。例如从一个段落跨到另一个段落，因此即使模型的阶数再提高也无可奈何。这就是马尔科夫假设的局限性，需要采用一些长程的依赖性（Long Distance Dependency）来解决
2. 用于平滑先验概率的Good-Turing Estimate
3. Katz backoff

## 第4章 谈谈中文分词

1. 机器翻译：分词颗粒度大较好
2. 网页搜索：粒度小较好

## 第6章 信息的度量和作用

1. 吴军&王作英：汉语信息熵和语言模型的复杂度
2. 传奇间谍：佐尔格
3. 合理利用信息，而不是玩弄什么公式和机器学习算法，是做好搜索的关键
4. 互信息可以用于NLP中消岐
5. 李开复的Sphinx语音识别系统
6. Thomas Cover: Elements of Information Theory

## 第9章 图论和网络爬虫

1. 对于一个url，先将域名提出并哈希，该url就由该哈希对应的机器负责下载
2. 爬虫节点之间的通信可通过batch方式实现

## 第12章 地图和本地搜索的最基本技术 - 有限状态机和动态规划

1. 对地址格式建立有限状态机模型，用于分析网页，找到网页中的地址部分，建立local search的database
2. 基于概率的有限状态机可用于解决地址的模糊匹配问题
3. Mehryar Mohri, Fernando Pereira, Michael Riley的通用概率有限状态机c语言库：<http://www.cs.nyu.edu/~mohri>
4. Weighted Finite State Transducer, WFST

## 第13章 Google AK-47的设计者 - 阿米特·辛格博士

1. Amit Singhal院士：Google的排序算法Ascorer

## 第14章 余弦定理和新闻的分类

1. 自底向上不断合并：按照新闻之间两两的余弦相似性，根据某个阈值合成一小类，再用小类的特征向量向上合成大类，不断做下去

## 第16章 信息指纹及其应用

1. 把每个网址映射成一个128bit的数用于爬虫的哈希表（由于冲突的概率极小故不用存储原来的网址）
2. 信息指纹还可用于判断集合基本相同：
   - 用于判断垃圾邮件账号
   - 网页去重（找出IDF最大的词作为特征）
   - YouTube反盗版：视频关键帧作为集合元素
3. 相似性哈希：
   - Moses Charikar, Similarity Estimation Techniques from Rounding Algorithms
   - Gurmeet Singh Manku, Arvind Jain and Anish Das Sarma, Detecting Near-Duplicates for Web Crawling

   原理：一组词t1, t2, …, tk及其权重w1, w2, …, wk，分别算出每个词的哈希值hi（设一共有B位），然后：
   1. 把hi扩展成B个数并让对应的数累加。对hi的第k位，若为1则加上wi，为0则减
   1. 把B个数收缩为一个B位的二进制数，第k个数若>0则第k位为1否则为0。

## 第18章 搜索引擎反作弊问题

1. 作弊的本质是在网页排名信号中加入了噪音，因此反作弊的关键是去噪音（根据通信原理、通信模型和信号处理的原理）
2. 解决方法：
   - 针对商业性的搜索，采用一套“抗干扰”强的搜索算法
   - 对信息类的搜索，采用“敏感”的算法
3. 每个网站到其他网站的出链数目作为一个向量，计算两个网站的出链向量的余弦值可以找出那些专门卖链接的网站。（余弦几乎为1）
4. 图论：用于发现clique

## 第19章 谈谈数学模型的重要性

1. 一个正确的数学模型应当在形式上是简单的
2. 一个正确的模型一开始可能不如一个精雕细琢过的错误模型来得准确，但若是对的就应坚持
3. 大量准确的数据对研发很重要
4. 正确的模型可能受噪音干扰而不准确，正确做法是找到噪音根源，这可能通往重大发现

## 第20章 谈谈最大熵模型

1. GIS (Generalized Iterative Scaling)算法
2. IIS (Improved Iterative Scaling)算法（达拉皮垂）

## 第25章 条件随机场和句法分析

1. 浅层分析、GParser
