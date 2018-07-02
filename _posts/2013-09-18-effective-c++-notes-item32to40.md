---
layout: post
title: 《Effective C++》笔记（条款32-40）
date: 2013-09-18 00:00:02 +0000
categories:
    - 程序&设计
tags:
    - c++
    - 设计模式
    - 读书笔记
---

* TOC
{:toc}

## 条款32 确定你的public继承塑模出is-a关系

1. 好的接口可以防止无效代码通过编译
2. 世界上并不存在一个“适用于所有软件”的完美设计，所谓最佳设计，取决于系统希望做什么事情，包括现在与未来
3. public继承主张：能够施行于base class对象身上的每件事情，也可以施行于derived class对象身上

## 条款34 区分接口继承与实现继承

1. pure virtual函数只具体指定接口继承
2. impure virtual函数具体指定接口继承及缺省实现继承
3. non-virtual函数具体指定接口继承以及强制性实现继承

## 条款35 考虑virtual函数以外的其他选择

1. strategy设计模式：将继承体系内的virtual函数替换为另一个继承体系内的virtual函数
2. tr1::function的对象的行为像一般函数指针，这样的对象可以接纳“与给定的目标签名式兼容”的所有可调用物（经常搭配tr1::bind使用）
3. non-virtual interface(NVI)手法：以public non-virtual成员函数包裹较低访问性的virtual函数。这是Template Method设计模式的一种特殊形式

## 条款36 绝不重新定义继承而来的non-virtual函数

1. 这也是多态性base classes内的析构函数应该是virtual的原因（条款7）

## 条款37绝不重新定义继承而来的（virtual函数的）缺省参数值

1. 因为缺省参数值都是静态绑定，而virtual函数是动态绑定。这可能导致开发者意想不到的后果。

## 条款38 通过复合塑模出has-a或“根据某物实现出”

1. 复合（composition）的同义词：layering（分层），containment（内含），aggregation（聚合），embedding（内嵌）
2. 应用域（application domine）：如真实世界的事物
3. 实现域（implementation domain）：如缓冲区、互斥器、查找树等
4. 在应用域，复合意味着has-a；在实现域，复合意味着is-implemented-in-terms-of（根据某物实现出）

## 条款39 明智而审慎地使用private继承

1. private继承时，编译器不会自动将一个derived class对象转换为一个base 对象（通过指针或引用）
2. 由private base class继承而来的所有成员，在derived class中都会编程private属性
3. private继承意味着implemented-in-terms-of，让D private继承B时是为了采用B内某些备妥的特征，而不是因为B和D存在任何观念上的关系
4. private继承纯粹只是一种实现技术，在软件“设计”层面上没有意义，其意义只及于软件实现层面
5. 和复合不同，private继承可造成空白基类最优化（empty base optimization，EBO），从而带来空间效率的提升

## 条款40 明智而审慎地使用多重继承

1. 从正确行为的观点看，public继承应该总是virtual；然而为了避免继承得来的成员变量重复，编译器必须提供若干机制，导致使用virtual继承的那些classes所产生的对象往往比使用non-virtual继承的兄弟们体积大。因此，用virtual继承实现虚基类是要付出一定代价的。
2. 多重继承的正当用途之一：“public继承某个interface class”和“private继承某个协助实现的class”的两相组合。
