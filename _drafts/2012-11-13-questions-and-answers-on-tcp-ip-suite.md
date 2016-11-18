---
layout: post
title: 一些关于TCP/IP协议族的疑问和解答
categories:
    - 网络&分布式
tags:
    - Work in Progress
    - TCP/IP
    - 协议
    - 网络
---

{::comment} Not show line_numbers {:/comment}
{::options syntax_highlighter_opts="{default_lang: c++ \}" /}

* TOC
{:toc}

1. 生成树协议（STP）：是否协议完成之后的所有帧转发都按照生成树的边来发？而其他链路就相当于没用了？这样当数据量暴增时单挑链路会不会成为性能瓶颈？
2. IP隧道是指什么东西？指Tunneling吗？
3. VPN的工作原理？
4. 当一个主机联网后都有哪些协议负责初始化和维护IP地址？ARP、ICMP、DHCP？
5. 路由器如何避免数据报传输发生回环？BGP、OSPF、RIP等工作原理是怎么样的？
6. 为什么面向连接的协议（如TCP）要固定路由？（TCP/IP详解卷1中文版p85）
7. 有没一些网络问题实例及其解决方案的书籍？
8. Ping一个广播地址不需要发送ARP吗？为什么？
9. TCP的重传定时器对于重传仍然丢失的情况是怎么处理的？
