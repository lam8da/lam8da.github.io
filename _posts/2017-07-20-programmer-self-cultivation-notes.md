---
layout: post
title: 《程序员的自我修养》读书笔记
categories:
    - 系统&体系结构
tags:
    - Linux
    - Windows
    - 操作系统
    - 编译原理
    - Work in Progress
    - 读书笔记
---

{::comment} Not show line_numbers {:/comment}
{::options syntax_highlighter_opts="{default_lang: c++ \}" /}

* TOC
{:toc}

- 参考文献
  - Linux内核源代码情景分析
  - 深入理解Windows操作系统 - Microsoft Windows Server 2003/Windows XP/Wincows 2000技术内幕
  - <span style="color: red">此书一共三卷，这是第一卷，还有两卷呢？？</span>
- singleton的实现：
  ```
  if (singleton_ptr = NULL) {
      lock();
      if (singleton_ptr = NULL) {
          singleton_ptr = new T();
      }
      unlock();
  }
  ```
  实际上是不安全的，因为语句`singleton_ptr = new T();`分三步：第一步分配内存，第二步在分配的内存上构造T，第三步把内存指针赋值给singleton_ptr。其中第二步和第三步可能被cpu乱序执行调换顺序，因为它们之间没有依赖。这样如果第三步先执行，另一线程并发访问，就会发现singleton_ptr被赋值了但是内容还没被构造。
- linux的clone函数
- 三种线程模型（“对”表示用户线程和内核线程的映射情况）：
  - 1对1：缺点是OS限制了内核线程数量从而导致用户线程数量受限，而且线程切换开销较大
  - 多对1：一个用户线程阻塞将导致所有线程阻塞，增加cpu个数也不管用
  - 多对多：在多处理器系统上多对多模型的性能提升幅度不如一对一模型高
- 汇编器as，链接器ld
- 词法分析器lex，语法分析器yacc
- 链接过程主要包括：地址和空间分配（Address and Storage Allocation）、符号决议（Symbol Resolution）和重定位（Relocation）
- 文件类型p57
  - 可重定位文件：windows的.obj和linux的.o
  - 动态链接库：windows的.dll和linux的.so
  - 静态链接库：windows的.lib和linux的.a
  - 可执行文件
  - 核心转储文件：linux的core dump

