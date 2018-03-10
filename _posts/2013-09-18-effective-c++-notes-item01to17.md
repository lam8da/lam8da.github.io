---
layout: post
title: 《Effective C++》笔记（条款01-17）
categories:
    - 程序&设计
tags:
    - c++
    - 设计模式
    - 读书笔记
---

* TOC
{:toc}

## 条款02 尽量以const，enum，inline替换#define

1. class中的static const常量只要不取其地址，就可以声明它而无须提供定义式（即在class定义的头文件中声明它并在声明中赋予初值），这称为“in-class”初值设定
2. 最常用的做法是声明式位于头文件内，定义式位于实现文件内，初值设定也位于实现文件内
3. 唯一例外是在class的编译期间需要一个class常量值（如用于确定数组成员的大小）。此时常使用“in-class”初值设定（只允许对整数常量进行）
4. 若旧式编译器不允许像3那样做，可以使用“the enum hack”。这也可以避免取一个常量地址。
5. 对于形似函数的宏，最好改用inline函数替换#define

## 条款03 尽可能使用const

1. 两个成员函数如果只是常量性（constness）不同，可以被重载
2. 当const和non-const成员函数有着实质等价的实现时，令non-const版本调用const版本可避免代码重复（需使用const-cast和static-cast）

## 条款04 确定对象被使用前已先被初始化

1. 解决不同编译单元的non-local static对象（包括global对象、定义于namespace作用域内的对象、在classes内、file作用域内被声明为static的对象）初始化次序的问题：将它们搬到自己的专属函数内，该对象在此函数内被声明为static，这些函数返回一个reference指向它所含的对象

## 条款06 若不想使用编译器自动生成的函数，就该明确拒绝

1. 为驳回编译器自动提供的机能，可将相应的成员函数声明为private并不予实现

## 条款07 为多态基类声明virtual析构函数

1. 如果class带有任何virtual函数，它就应该拥有一个virtual析构函数

## 条款08 别让异常逃离析构函数

1. 析构函数绝对不要吐出异常！

## 条款09 决不在构造和析构过程中调用virtual函数

1. base class构造期间virtual函数绝不会下降到derived class阶层，或，在base class构造期间，virtual函数不是virtual函数

## 条款11 在operator=中处理“自我赋值”

1. 确保当对象自我赋值时operator=有良好的行为，可以使用copy-and-swap方法实现

## 条款17 以独立语句将newed对象置入智能指针

1. 否则new操作符之后可能被编译优化器插入一些会异常的语句而导致资源泄露
