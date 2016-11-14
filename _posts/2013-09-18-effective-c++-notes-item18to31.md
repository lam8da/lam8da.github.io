---
layout: post
title: 《Effective C++》读书笔记（条款18-31）
categories:
    - 程序&设计
tags:
    - c++
    - 设计模式
    - 读书笔记
---

* TOC
{:toc}

## 条款18 让接口容易被正确使用，不易被误用

1. cross-DLL problem

## 条款22 将成员变量声明为private

1. protected并不比public更具封装性

## 条款23 宁以non-member、non-friend替换member函数

1. 若一个函数可以独立于一个class的被封装成员而存在，则宁将其变成non-member、non-friend函数而不是member函数，这样做可以增加封装性、包裹弹性（packaging flexibility）和机能扩充性（越多东西被封装，就越少人可以看见它，我们就有越大的弹性去改变它）。

## 条款24 若所有参数皆需类型转换，请为此采用non-member函数

1. 如果你需要为某个函数的所有参数（包括被this指针所指那个隐喻参数）进行类型转换，那么这个函数必须是个non-member
2. member的反面是non-member，不是friend。无论何时如果你可以避免friend就该避免，不能够只因函数不该成为member，就自动让它成为friend

## 条款25 考虑写出一个不抛异常的swap函数

1. 当std::swap对你的类型效率不高时（那几乎总是意味着你的class或template使用了某种pimpl（pointer to implementation）手法），提供一个swap成员函数，并确定这个函数不抛出异常
2. 如果你提供一个member swap，也该提供一个non-member swap用来调用前者。对于classes（而非template），也请特化std::swap。
3. 调用swap时应针对std::swap使用using声明式，然后调用swap并且不带任何“命名空间资格修饰”。
4. 不要尝试重载std::swap。
5. 成员版swap绝不可抛出异常！因为高效率（这一般也是你提供一个member swap的原因）几乎总是基于对内置类型的操作，而内置类型上的操作绝不会抛出异常！

## 条款27 尽量少做转型动作

>  转型总是伴随着临时对象的产生？
   {: .lambda_question}

1. 若转型是必要的，使用c++ style的转型，并将其隐藏于某个函数的背后，而不是让客户把它们放到自己的代码中。

## 条款28 避免返回handles指向对象的内部成分

1. 包括references、指针、迭代器等

## 条款29 为“异常安全”而努力是值得的

1. 异常安全函数即使发生异常也不会泄露资源或允许任何数据结构败坏。三种可能性保证：
   1. 基本保证：所有对象都处于内部一致的状态中，但程序的现实状态不可预料
   1. 强烈保证：类似于原子操作：若异常抛出，程序状态不改变
   1. 不抛掷保证：承诺绝不抛出异常
2. 强烈保证往往能以copy-and-swap实现出来，但并非对所有函数都可实现或具备现实意义
3. 以对象管理资源
4. 没有所谓“局部/部分异常安全性”
5. 函数提供的“异常安全保证”通常最高只等于其所调用的各个函数的“异常安全保证”中的最弱者

## 条款30 透彻了解inlining的里里外外

1. 隐喻方式的inline：将函数定义于class定义式内
2. inline函数通常一定要置于头文件内
3. inline只是个请求，编译器可以拒绝：如对所有virtual函数的调用绝不会被inline
4. 取inline函数本地的地址可能导致编译器为其生成一个outlined函数本体
5. 编译器通常不对“通过函数指针而进行的调用”实施inlining
6. inline函数无法随着程序库的升级而升级
7. 构造函数和析构函数往往是inlining的糟糕候选人

## 条款31 将文件间的编译依存关系降至最低

1. 支持“编译依存性最小化”的一般构想是：相依于声明式，不要相依于定义式。基于此构想的两个手段：
   - Handle classes：为声明式和定义式提供不同的头文件，使用pimpl idiom，如果使用object references或object pointers可以完成任务，就不要使用objects
   - Interface classes：基类只提供接口，使用工厂函数生成子类对象
