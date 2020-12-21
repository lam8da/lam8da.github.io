---
layout: post
title: 《Fourier Analysis》笔记
categories:
    - 数学&物理
tags:
    - Work in Progress
    - 读书笔记
    - 数学  
---

{::comment} Not show line_numbers {:/comment}
{::options syntax_highlighter_opts="{default_lang: c++ \}" /}

* TOC
{:toc}

# Chapter 1. The Genesis of Fourier Analysis

- 简谐振动（基于弹簧）
  - 力$$F=-ky(t)$$，$$k$$表示弹性系数，$$t$$为时间
  - 应用牛顿第二定律后得$$-ky(t)=my''(t)$$，可简化成$$y''(t)+c^{2}y(t)=0$$的形式
  - 该微分方程的一般解为$$y(t)=a\cos(ct)+b\sin(ct)$$，且该解为唯一二次可微的解
    > 如何证明？
      {: .lambda_question}
- 求解匀质弹性线段的波动方程$$u(x,t)$$
  - 该线段两端点坐标为$$x=0$$和$$x=L$$
  - 分为$$N$$段，每段长度为$$h$$
  - 假设线段密度为$$\rho$$，根据牛顿第二定律，第$$n$$段所受作用力可以表示为$$\rho hy_{n}''(t)$$
  - 设$$\tau>0$$为线段的张力系数（常数），第$$n$$段所受右边（左边类似）相邻段的张力和$$(y_{n+1}-y_{n})/h$$成正比，这样该段所受左右两边的张力和为

    $$
    \frac{\tau}{h}\{y_{n+1}(t)+y_{n-1}(t)-2y_{n}(t)\}
    $$

    其中$$y_{n}(t)=u(x_{n},t)$$
  - 联立上述两式得

    $$
    \rho hy_{n}''(t)=\frac{\tau}{h}\{y_{n+1}(t)+y_{n-1}(t)-2y_{n}(t)\}
    $$
  - 令$$h\to0$$取极限有

    $$
    \rho\frac{\partial^{2}u}{\partial t^{2}}=\tau\frac{\partial^{2}u}{\partial x^{2}}
    $$

    这就是one-dimentional wave equation
- 求解上述wave equation的两种方法
  - 
  

# Chapter 2. Basic Properties of Fourier Series

# Chapter 3. Convergence of Fourier Series
