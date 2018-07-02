---
layout: post
title: 《深入理解计算机系统》笔记
date: 2017-07-13 00:00:00 +0000
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

- 2.2.7 c语言规定，像由short→unsigned int这类型的转换，都是先扩充字节（大小），再改变符号
- 2.3.7 补码负数x右移k位等于[x/2^k]（[]表示向下取整），但根据规则来说应该是向上取整才对。可以通过{x/y}=[(x+y-1)/y]（{}表示向上取整）来达到这个目的
- 2.4.3 以IEEE754格式表示的非NaN的两个正（负）浮点数的大小顺序与其位模式对应的无符号数的大小顺序是相同（相反）的！！！
- 2.4.5 浮点数的加法不遵循可结合性，因此某些时候编译器不会进行某些理论上可行的实数运算优化。
  - 浮点加法满足可结合性：若a>=b则对于任意非NaN的x有a+x>=b+x
  - 浮点乘法满足单调性：若a>=b且c>=0则a*c>=b*c，若a>=b且c<=0则a*c<=b*c
- 3.? leave指令等价于指令组合：movl %ebp %esp; popl %ebp
- 3.? call ADDR等价于（不合法的）指令组合：pushl %eip; movl ADDR %eip
- 3.? ret指令等价于：popl %eip
- 3.? 在IA32中，call ADDR后，该call指令的下一条指令地址会被压栈，而ADDR处的前几条指令必须为：pushl %ebp; movl %esp %ebp。这与leave刚好相反
- 3.? 在IA32中，调用者保存寄存器包括eax, edx, ecx；被调用者保存寄存器包括ebx, esi, edi
- 3.? IA32中：Linux的对齐策略是，2字节类型如short的地址必须是2的倍数，4字节以上必须为4的倍数；windows则有更强的要求，N字节类型的地址必须为N的倍数（N为2，4，8）；SSE要求的地址必须是16的倍数。
- 3.? IA32惯例：确保每个栈帧的长度是16字节的整数倍
- 3.? 防止buffer overflow的方法：
  - 程序地址随机化
  - 用随机产生的canary（金丝雀传说能吸去毒素）值作返回前校验
  - 限制可执行的内存页，让栈所在的页不可以被执行
- 3.? 文献：81，94；Blum的书9；Application Binary Interface；Muchnick的76；蠕虫102，40；缓冲区冲击94
- 4 文献：Katz的逻辑设计教科书[56]
- 4 SEQ和PIPE的HCL书附源代码
- 寄存器重命名；时间局部性和空间局部性
- 高效矩阵转置和矩阵乘法：[link1](http://edisonx.pixnet.net/blog/post/91005914-%5Bc%E8%AA%9E%E8%A8%80%E6%95%B8%E5%80%BC%E5%88%86%E6%9E%90%5D-%E7%9F%A9%E9%99%A3%E4%B9%98%E6%B3%95-%3C-cache-block-%3E)，[link3-高逸函](http://fanhq666.blog.163.com/blog/static/8194342620115127329901/)。<span style="color: red">对于乘法，可以先对乘数矩阵做转置再做，这个与按缓存大小分块方法（不转置直接乘）哪个更快？</span>
- <span style="color: red">习题6.46：优化矩阵转置</span>
  - 方法1：每次处理一个4*4的小矩阵，因为两个（source and dest）该规模的小矩阵的大小为2*4*4*4=128B能够完全放入L1 cache，所以交换16个元素只会有8次cache miss。见[这里](http://blog.csdn.net/yang_f_k/article/details/9499359)
- <span style="color: red">习题6.47：优化矩阵的如下操作：f[i][j] = f[i][j] || f[j][i] for each i, j</span>
  - 方法1：类似于上题的方法1，见[这里](http://blog.csdn.net/yang_f_k/article/details/9500225)
- 命令：c预处理器cpp，c编译器cc1，汇编器as，链接器ld
- GNU的binutils
- 第七章参考：Levine的专著[66]；[52]；二进制翻译Valgrind；Atom[103]
- c++/Java异常是通过setjmp/longjmp实现的？catch类比于setjmp，throw类比于longjmp
- strace命令打印一个正在运行的程序和它的子进程调用的每个系统调用的轨迹
- pmap命令显示进程的存储器映射
- /proc：cat /proc/loadavg显示linux系统的平均负载
- 第八章参考：W.Richard Stevens[110]，Bovet&Cesati[11]linux内核，Blum[9]关于x86汇编

