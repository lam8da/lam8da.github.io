---
layout: post
title: 《Unix痛恨者手册》笔记
categories:
    - 程序&设计
tags:
    - 读书笔记
    - Unix
    - Linux
    - Work in Progress
---

{::options syntax_highlighter="rouge" /}
{::options
  syntax_highlighter_opts="{
    default_lang: sh,
    css_class: highlight,
    span: {
      line_numbers: false
    \},
    block: {
      line_numbers: true,
      start_line: 1
    \}
  \}"
/}

一些经验总结以及好玩的段子摘录：

- 请千万别让人用“安全”命令去替换标准命令。
  1. 许多shell程序会对多嘴的`rm`感到惊讶，而且也不会想到删除了的文件仍然占有磁盘空间。 
  1. 并不是所有删除操作都是安全的，有户会因此产生一切都能恢复的错觉。 
  1. 那些不标准的命令对系统管理员来说尤其可恨。如果你想有个有确认功能的“`rm`”，用下面的命令: `% alias del rm -i`

  千万别替换`rm`!
- 有人整理了一些UNIX最为可笑的错误信息，把他发布在Usenet上。他们使用的是C shell.

  ```
  % rm meese-ethics 
  rm: messe-ethics nonexistent
  
  % ar m God 
  ar: God does not exist
  
  % "How would you rate Dan Quayle's incompetence? 
  Unmatched ".
  
  % ^How did the sex change^ operation go? 
  Modifier failed.
  
  % If I had a ( for every $ the Congress spent, what would I have? 
  Too many ('s
  
  % make love 
  Make: Don't know how to make love. Stop.
  
  % sleep with me 
  bad character
  
  % got a light? 
  No match
  
  % man: why did you get a divorce? 
  man: Too many arguments.
  
  % ^What is saccharine? 
  Bad substitute.
  
  % %blow 
  %blow: No such job.
  ```
  
  下面的这些幽默作品来自Bourne Shell:
 
  ```
  $ PATH=pretending! /usr/ucb/which sense 
  no sense in pretending
  
  $ drink <bottle; opener 
  bottle: cannot open 
  opener: not found
  
  $ mkdir matter; cat >matter 
  matter: cannot create
  ```
- Don Libes和Sandy Ressler在《UNIX生活》(Life with UNIX)中对UNIX哲学作了很好的总结：
  - 小即是美 
  - 用10%的工作解决90%的任务 
  - 如果必须作出选择，选择最简单的那个。
- 根据UNIX程序和工具的实际表现来看，对UNIX哲学更为精确的总结应该是:
  - 小的程序比正确的程序更好 
  - 粗制滥造是可以接受的 
  - 如果必须作出选择，选择责任最小的那个。
- 大多数操作系统上的“kill”仅仅用来杀死进程。但UNIX的设计更为通用：“kill”被用来向进程发送一个信号，这体现了UNIX的一个设计原则：尽量使操作通用，给予用户强大力量(power)
- 这又体现了一个UNIX设计原则：对于给用户的反馈，能少说绝不多说，能晚说绝不早说。以免过多信息所可能导致的用户脑损伤。
- 有人开玩笑说：“如果你觉得需要阅读关于内核函数的文档，那么很可能你本来就不配使用这些函数。”
- 所有读过UNIX代码的人都被下面的一行粗暴注释惊呆过：

  ```c++
  /* you are not expected to understand this */ （/* 没指望你能明白 */）
  ```
- 如何知道运行正确：没有消息就是好消息
- GPL的好处在于你不必为自己的工作负责，也不必对用户负责
- “有些操作系统从没有被好好计划，以至于只好用反刍的噪音来命名它的命令（awk, grep, fsck, norff），我想到这个就反胃。” —— 无名氏
- 这七种针对shell命令行的转换方式是：
  - 别名：`alias, unalias`
  - 命令输出替代：`` ` ``
  - 文件名替代：`*, ?, []`
  - 历史替代：`!, ^`
  - 变量替代：`$, set, unset`
  - 进程替代：`%`
  - 引用：`', "`

  这一“设计”的结果是，问号(?)永远被shell当成单字符匹配符，它永远无法被作为命令行参数传给用户程序，所以别想使用问号来作为帮助选项。
- `"${1+"$@"}"`究竟是什么意思：这是`/bin/sh`里用来完整复制命令行参数的一种方法。它的意思是：如果有至少一个参数(`${1+`)，那么用所有参数(`"$@"`)来替代以保留所有的空白字符。如果我们只使用`"$@"`，那么在没有参数的情形下会得到`""`而不是我们想要的空参数。
- 那么，为什么不使用`"$*"`呢？sh(1)的man手册是这样说的：双引号之间的参数和命令会被替换，shell会对结果加上引用，以避免解析空格或生成文件名。如果`$*`出现在双引号之中，各个参数之间的空格会被加上引用(`"$1 $2 ..."`)，而如果`$@`出现在双引号之中，各个参数之间的空格不会被加上引用(`"$1" "$2" ...`)。
- 如果我写着不容易，那么你理解起来就不应该容易。 —— 一个Unix程序员
- Unix程序员总是打着“这会破坏已有代码”的幌子，不愿意修正bug。可这里面还有内幕，修正bug不但会破坏已有代码，还必须修改简单完美的Unix接口，而这正是Unix教众们的命根子。至于这个接口是否工作，这并不重要。Unix教众们不去提出更好的接口，也不去修正bug，而是齐声高唱“Unix接口好简洁，好简洁。Unix接口就是美，就是美！Unix无罪！Unix有理！”。不幸的是，绕过bug是个很恶劣的行为，它使得错误成为了操作系统规范的一部分。你越是等，就越难以修正，因为越来越多的程序会尽力绕过bug，以至于没有了bug反而活不了了。同理，修改操作系统接口带来的影响更大，因为更多的程序必须根据这个正确的新接口进行修改。(这解释了为什么ls有那么多的选项来完成几乎一样的工作)。
- 如果你把一只青蛙仍到开水里，它会马上跳出来。它知道开水很烫。可是，如果你把青蛙放到冷水里，再慢慢地加热，青蛙感觉不到什么，直到最后被烫死。Unix接口已经开锅了。以前，输入/输出的全部接口只包括open, close, read和write。网络支持给Unix添了一大把柴禾。现在，至少有五种方法向一个文件句柄输入数据：write, writev, send, sendto和sendmsg。每个都在内核中有不同的实现，这意味着有五倍的可能出现bug，有五种不同的性能结果需要考虑。读文件也一样(read, recv, recvfrom和recvmsg)。等死吧，青蛙们。
- 问："C"和"C++"的名字是怎么来的？答：这是他们的成绩 —— Jerry Leichter。再没有比C++更能体现Unix“绝不给用户好脸”的哲学思想的了。
- 面向对象的汇编语言（注：指C++）
