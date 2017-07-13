---
layout: post
title: 《Linux内核设计与实现》（中+英）读书笔记
categories:
    - 系统&体系结构
tags:
    - Linux
    - 内核
    - 操作系统
    - Work in Progress
    - 读书笔记
---

{::comment} Not show line_numbers {:/comment}
{::options syntax_highlighter_opts="{default_lang: c++ \}" /}

* TOC
{:toc}

- 3.2.1 进程的thread_info结构存放于它们内核栈的尾端（最低地址），其task成员指向task_struct。这是由于x86没有足够的寄存器
- 3.2.3 进程状态
- 3.2.5 进程上下文（内核空间），中断上下文
- 3.3.2 fock(), vfork(), __done()都调用clone()，而clone()调用do_fork()
- 3.4.2 内核线程：没有独立的地址空间（mm指针置为NULL）
- 3.? 父进程先于子进程退出：先在父进程的线程组中找，若找不到就会令init为子进程的继父进程
- 基于优先级的调度
  - nice值取值区间为-20到19，代表时间片的比例，越小优先级越高
  - 实时优先级：0至99，越大优先级越高

