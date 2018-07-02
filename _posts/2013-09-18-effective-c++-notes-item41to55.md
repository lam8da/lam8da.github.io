---
layout: post
title: 《Effective C++》笔记（条款41-55完）
date: 2013-09-18 00:00:03 +0000
categories:
    - 程序&设计
tags:
    - c++
    - 设计模式
    - 读书笔记
---

* TOC
{:toc}

## 条款42 了解typename的双重意义

1. 从属名称（dependent names）：template内出现的名称相依于某个template参数
2. 嵌套从属名称（nested dependent name）：在class内呈嵌套状的从属名称
3. 嵌套从属类型名称（nested dependent type name）：指涉某类型的嵌套从属名称
4. 如果解释器在template中遭遇一个嵌套从属名称，它总是假设这名称不是个类型，除非你告诉它是
5. typename不可以出现在base classes list内的嵌套从属类型名称之前（即如果基类是一个嵌套类模板，则该基类的名字是一个嵌套从属类型名称，但在派生类中继承该基类的地方不能用typename）；也不可以在member initialization list（成员初值列）中作为base class修饰符。

## 条款43 学习处理模板化基类内的名称

1. 如果一个基类是个模板，那么派生类模板在定义时是不知道它继承的是什么样的class的（要在实例化之后才知道基类里有什么成员/名称），这样在派生类内无法直接使用基类的名称。解决方法：可在derived class templates内通过“this->”指涉base class templates内的成员名称，或使用using声明式将其带入作用域内，或籍由一个明白写出的“base class资格修饰符”完成（但这会关闭“virtual绑定行为”，是最不让人满意的）。

## 条款44 将与参数无关的代码抽离template

1. 以函数参数或class成员变量替换template参数可以消除因非类型模板参数造成的代码膨胀
2. 让带有完全相同二进制表述的具现类型共享实现码可降低因类型参数而造成的代码膨胀

## 条款45 运用成员函数模板接受所有兼容类型

1. member templates并不改变语言规则，而语言规则说，如果程序需要一个copy构造函数你却没有声明它，编译器会为你暗自生成一个。在class内声明泛化copy构造函数（是个member template）并不会阻止编译器生成它们自己的copy构造函数（一个non-template）。因此如果你想要控制copy构造函数的方方面面，你必须同时声明泛化copy构造函数和“正常的”copy构造函数。相同的规则也适用于赋值（assignment）操作。

## 条款46 需要类型转换时请为模板定义非成员函数

1. template在实参推导过程中从不将“通过构造函数而发生的”隐式类型转换纳入考虑
2. 当我们编写一个class template，而当它提供的“与此template相关的”函数支持“所有参数的隐式类型转换”时，请将那些函数定义（注意是定义，即要在该class template中给出实现）为“class template”内部的friend函数
3. 这样，我们虽然使用friend，却与friend的传统用途“访问class的non-public成分”毫不相干。为了让类型转换可能发生于所有实参身上，我们需要一个non-member函数（条款24）；为了令这个函数被自动具现化，我们需要将它声明在class内部；而在class内部声明non-member函数的唯一办法就是：令它成为一个friend。

## 条款47 请使用traits classes表现类型信息

5步设计并实现一个traits class：

1. 确认若干你希望将来可取得的类型相关信息（如对迭代器而言，我们希望可取得其分类（category）
1. 为该信息选择一个名称（如iterator\_category）
1. 提供一个template和一组特化版本（如iterator\_traits），内含你希望支持的类型相关信息
1. 简历一组重载函数（身份像劳工）或函数模板，彼此间的差异只在于各自的traits参数，令每个函数实现码与其接受的traits信息相对立
1. 建立一个控制函数（身份像工头）或函数模板，它调用上述那些“劳工函数”并传递traits class所提供的信息

## 条款48 认识template元编程（TMP）

1. TMP主要是个函数式语言，已被证明是个“图灵完全”机器
2. 通过所谓expression templates技术可以优化矩阵运算
3. TMP可以生成客户定制的设计模式实现品。该技术已超越编程工艺领域（如设计模式和智能指针），更广义地成为generative programming（殖生式编程）的一个基础

## 条款49 了解new-handler的行为

1. 一个设计良好的new-handler函数必须做以下事情：
   1. 让更多内存可被使用
   1. 安装另一个new-handler
   1. 卸除new-handler
   1. 抛出bad\_alloc（或派生自bad\_alloc）的异常
   1. 不返回：调用abort或exit
2. 怪异的循环模板模式（curiously recurring template pattern; CRTP）

## 条款51 编写new和delete时需固守常规

1. operator new内含一个无穷循环（自己编写的版本也应该这么做），退出此循环的唯一办法是：内存被成功分配或new-handler函数做了一件描述于条款49的事情。
2. operator new也应该有能力处理0 bytes的身躯，而class专属版本还应该处理“比正确大小更大的（错误）申请”（来自派生类的调用）。
3. operator delete应该在收到null指针时不做任何事。class专属版本还应该处理“比正确大小更大的（错误）申请”。

## 条款52 写了placement new也要写placement delete

1. 如果operator new接受的参数除了一定会有的那个size\_t之外还有其他，这边是所谓的placement new。众多placement new版本中特别有用的一个是“接受一个指针指向对象该被构造之处”的那个。
2. 类似于new的placement版本，operator delete如果接受额外参数便成为placement deletes。placement delete只有在“伴随placement new调用而触发的构造函数”出现异常时才会被调用。对一个指针施行delete绝不会导致调用placement delete。
   > 所以那个有个size\_t参数的delete不是placement的？
     {: .lambda_question}
3. 当你自己写一个placement operator new，请确定也写出了对应的接受相同额外参数的placement operator delete。否则编译器在当构造函数发生异常的地方不知道调用哪个delete，从而会什么也不做而导致内存泄露。
4. 缺省情况下C++在global作用域内提供了3个operator new（一个最normal的，一个特别版placement的，一个nothrow的）。如果你在class内声明任何operator news，它会遮掩这些标准形式。除非你的意思就是要这么干，否则请确保它们在你所生成的任何定制型operator new之外还可用。
