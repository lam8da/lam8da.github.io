---
layout: post
title: 《C++ Primer》读书笔记（第17-18章完）
categories:
    - 程序&设计
tags:
    - c++
    - 读书笔记
---

1. TOC
{:toc}

## 第17章 用于大型程序的工具

1. 承诺不抛出任何异常的方法：void func() throw() {}

2. 命名空间别名（像定义变量一样）：namespace n1 = n2;  // n2是一个已定义的命名空间

3. 屏蔽命名空间名字规则的一个重要例外：接受类类型形参（或类类型指针及引用形参）的函数（包括重载操作符），以及与类本身定义在同一命名空间中的函数（包括重载操作符），在用类类型对象（或类类型的引用及指针）作为实参的时候是可见的。如：
std::string s;
getline(std::cin, s);  // ok
当compiler看到getline(std::cin, s);时，它在当前作用域、包含调用的作用域以及定义cin和string的命名空间中查找匹配的函数。

4. 在最底层派生类的构造函数中，c++会首先初始化虚基类；在初始化中间层派生类时，忽略它们对虚基类的初始化。

 第18章 特殊工具与技术

1. c++提供以下两种方法分配和释放未构造的原始内存：allocator类；标准库中的operator new和operator delete。

2. c++还提供不同的方法在原始内存中构造和撤销对象：
a. allocator类的construct和destroy成员
b. 定位new表达式（placement new expression）接受指向未构造内存的指针，并在该空间中初始化一个对象或一个数组
c. 可直接调用对象的析构函数来撤销对象。运行析构函数并不释放所在内存
d. uninitialized_fill和uninitiated_copy

3. operator new/delete接口（只分配/释放空间，不构造/析构）：
void *operator new(size_t);
void *operator new[](size_t);
void operator delete(void*);
void operator delete[](void*);

4. 定位new表达式（在已分配但未构造的内存中初始化对象）：
new (place_address) type
new (place_address) type (initializer_list)
其中place_address是指针，initializer_list是初始化列表。
例如：
allocator<T> alloc;
alloc.construct(ptr, t);  // 复制构造
可以改为：
new (ptr) T(t);

5. allocator的construct只能使用复制构造，但定位new表达式可以使用任何构造函数

6. allocator的方法和低级操作的对应关系：
allocate       <==> operator new
deallocate  <==> operator delete
construct   <==> 定位new表达式
destroy       <==> 直接调用析构函数如：ptr->~T();

7. 对于语句string *sp = new string(“initialized”);实际上发生三件事：
a. 调用operator new分配内存。如果operator new没被重载，就调用标准库中的operator new。
b. 运行该类型的一个构造函数，在a分配的内存中初始化之
c. 返回该对象的指针
而delete sp;实际上发生两件事：
a. 对sp指向的对象运行适当的析构函数
b. 调用名为operator delete的标准库函数释放内存

8. 我们可以为一个类C定义它特定的operator new/delete（称为“成员operator new/delete”）或operator new[]/delete[]函数，改变new表达式的行为：
a. C *obj = new C(); delete obj;  // 调用自定义版本
b. C *obj1 = ::new C(); ::delete obj1;  // 调用标准库版本

9. 成员operator delete有两种定义：
void C::operator delete(void *p);
void C::operator delete(void *p, size_t sz);
其中void*形参由delete表达式用被delete的指针初始化，size_t由编译器用第一个形参所指对象的字节大小自动初始化。size_t一般用于继承层次，根据virtual ptr指向的对象的大小确定。
类似地，成员operator delete[]也有两种定义。

10. 成员operator new/delete函数隐式地为静态函数，不必显式地将它们声明为static，虽然这样做是合法的。

11. 运行时类型识别（RTTI）：
I. typeid操作符：返回动态类型信息（当类型有虚函数时）或静态的、编译时的类型信息（以type_info对象的形式返回）
II. dynamic_cast操作符的用法：
a. 作用于指针

if (Derived *dptr = dynamic_cast<Derived*>(base_ptr)) {
  …
} else {
  …
}

b. 作用于引用

try {
  Derived &d = dynamic_cast<Derived&>(b);
  …
} catch (std::bad_cast) {
  …
}

12. 成员指针（pointer to member）：

a. 数据成员的指针：Type C::*ptr = &C::obj;
b. 成员函数的指针：Type (C::*ptr)(arg_type_list) = &C::func;
c. 类的static成员不需要特殊语法来指向，其指针就是普通指针
成员指针的使用（.*和->*操作符）：
C cobj, *cptr;
(cobj.*ptr)(arg_list);
(cptr->*ptr)(arg_list);

13. 关于嵌套类：
a. 嵌套在类模板内部的类是模板
b. 在外围类外部定义嵌套类的方法：
class Outer { class Inner;  /* forward declaration */ };
class Outer::Inter { /* definition */ };

14. 固有的不可移植的特性包括：位域、volatile限定符、链接指示（如extern “C”等）。
