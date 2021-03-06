---
layout: post
title: 「趣题」某IT公司一年最多上班时间的期望
categories:
    - 算法&数学
tags:
    - 算法
    - 概率
    - 面试题
---

今天又从笔完某宝公司的同学那听来一道比较有趣的笔试题，大意如下：一公司计划招\\(n\\)名员工，该公司有个员工福利就是，如果一年中的某一天是某个员工的生日，那么那天就全员放假，否则不放假（所有法定节假日以及周末都不放），问招多少人时全公司一年上班的有效时间最长？（有效时间定义为不放假的天数*员工数，看来公司都是资本家开的啊，就是要不顾一切剥削挨踢民工，神马福利都是坑爹的。。。）

一眼看过去，这就是一个算期望的题目。假设招\\(n\\)个人时该公司全年刚好工作\\(i\\)天的概率是\\(p(n,i)\\)，那么期望全年工作的时间就是

$$ E=n\cdot\sum_{i=0}^{\min(n,365)}i\cdot p(n,i) $$

然后就开始纠结这个\\(p(n,i)\\)怎么算。。。纠结完了之后还得求出\\(E\\)的最终表达式，然后还得求其最值以及取得最大值时的\\(n\\)的取值。。。。。

实际上，如果真的像上面这样算会悲剧的。想起具体数学离散概率那章以及算法导论上介绍的指示函数求期望的方法，可以换一种方法来算这题。

设\\(p_i\\)为全年第\\(i\\)天需要上班的概率，\\(E_i\\)为全年第\\(i\\)天要上班的天数的期望。则有

$$ p_i=\frac{364^n}{365^n} $$

$$ E_i=p_i\cdot 1 $$

{::comment}由于不同的日期要上班的期望天数是独立事件，{:/comment}
于是全年要上班的总时间的期望为

$$ E=n\cdot\sum_{i=1}^{365}E_i=365n\cdot(\frac{364}{365})^n $$

目测上述函数是\\(n\\)的单峰函数且有最大值，于是求导并令导数为0，求得当\\(n=-\frac{1}{\ln(\frac{364}{365})}\\)时\\(E\\)最大。用calc算了一下此时\\(n\\)是364.5，全年要上班的天数是134.28天。公司还是很人道的 :)
