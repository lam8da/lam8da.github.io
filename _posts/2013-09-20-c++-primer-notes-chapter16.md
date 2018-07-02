---
layout: post
title: 《C++ Primer》笔记（第16章）
date: 2013-09-20 00:00:05 +0000
categories:
    - 程序&设计
tags:
    - c++
    - 读书笔记
---

* TOC
{:toc}

{::comment} Not show line_numbers {:/comment}
{::options syntax_highlighter_opts="{default_lang: c++ \}" /}

## 第16章 模板与泛型编程

1. 模板形参可以是表示类型的类型形参（type parameter，表示未知类型），也可以是表示常量表达式的非类型形参（nontype parameter，表示未知值）
2. 函数模板也可以声明为`inline`。此时`inline`关键字放在模板形参表之后、返回类型之前，不能放在关键字`template`之前：

   ```
   template <typename T> inline T min(const T&, const T&);
   ```
3. 模板形参遵循常规名字屏蔽规则：与全局作用域中声明的对象、函数或类型同名的模板形参会屏蔽全局名字
4. 用作模板形参的名字不能在模板内部重用
5. 模板也可以只声明而不定义
6. 若模板定义中使用了模板形参中的类型的一个内部定义的类型，必须用`typename`显式指明该名字是一个类型，否则编译器会假定该名字指定的是一个数据成员
7. 当一个模板类型用作（同一函数的）一个以上函数形参类型时，对应的实参的类型必须完全一致（当通过“`<>`”提供显式实参时，或者属于下列情况时，例外），因为一般而言不会转换实参以匹配已有的实例化，相反，会产生新的实例。例外的情况为：
   - `const`转换：非`const`对象的引用或指针->`const`引用或指针；`const`或非`const`对象->非引用类型（忽略`const`）
   - 数组或函数到指针的转换。当模板形参不是引用类型时：数组/函数->指针。

   例：

   ```
   template<typename T> T fobj(T, T);
   template<typename T> T fref(const T&, const T&);
   string s1;
   const string s2;
   fobj(s1, s2);  // fobj(string, string); const is ignored
   fref(s1, s2);  // non const -> const reference
   int a[10], b[42];
   fobj(a, b);  // fobj(int*, int*);
   // fref(a, b);  // error!
   ```
8. 可以使用函数模板对函数指针进行初始化或赋值：

   ```
   template<typename T> int compare(const T&, const T&);
   int (*pf)(const int&, const int&) = compare;  // 实例化
   ```
   如果不能从函数指针类型确定模板实参，就会出错
9. 某些情况下不可能推断出模板实参的类型。当函数的返回类型必须与形参表中所用的所有类型都不同时，最常出现这一问题。可以采用“增加一个模板形参以表示返回类型，调用时显示指定该类型”的方法解决
10. 显式提供模板实参时可以像函数的默认实参机制那样省略右边的模板实参（由编译器推断）
11. 模板要进行实例化时，编译器必须能够访问定义模板的源代码。如果采用分别编译模型（separate compilation model），在模板定义时需要使用`export`关键字。
12. 类模板的成员函数只有为程序所用才进行实例化，如果某函数从未使用，则不会实例化该成员函数。一个典型例子是，若某类型没有定义默认的构造函数，也可以用`std::vector`存放它，但`vector`初始化时不能使用`vector`的只有一个`size`成员的构造函数。
13. 类模板中的友元声明：

    ```
    template<class T> class A;
    template<class T> class B {
     public:
      friend class A<T>;  // ok: A is known to be a template
      friend class C;  // ok: C must be an ordinary, non-template class
      template<class S> friend class D;  // ok: D is a template
      friend class E<T>;  // error: E wasn’t declare as a template
      friend class F<int>;  // error: F wasn’t declare as a template
    };
    ```
14. 在类特化的外部定义该特化类的成员时，成员之前不能加`template<>`标记
15. 可以只特化成员而不是特化整个类
16. 类模板的部分特化：特化某些模板形参而非全部。当声明了部分特化时，编译器将为实例化选择最特化的模板定义
17. 函数匹配与函数模板：若重载函数中既有普通函数又有函数模板，确定函数调用的步骤如下：
    1. 建立候选函数集，包括
       - 与被调函数名字相同的任意普通函数
       - 任意函数模板实例化（模板实参推断发现了与调用中所用函数实参相匹配的模板实参）
    2. 确定哪些普通函数是可行的（见本笔记7.22或原书7.8节关于如何选择可行函数的讨论）。候选集中每个模板实例都是可行的，因为模板实参推断保证了这点。
    3. 若需要转换来进行调用，根据转换的种类排列可行函数（注意模板函数的实例所允许的转换是有限的）
       - 如果只有一个函数可选，就调用之
       - 如果调用有二义性，从可行函数集合中去掉所有函数模板实例
    4. 重新排列去掉函数模板实例的可行函数。如果只有一个函数可选，调用之；否则有二义性。

    - 例一：

      ```
      /* I. */ template<typename T> int compare(const T&, const T&);
      /* II.*/ int compare(const char*, const char*);
      ```
      - 调用1：

        ```
        const char const_arr1[] = "world", const_arr2[] = "hi";
        compare(const_arr1, const_arr2);
        ```
        将调用普通函数II。将`T`绑定到`const char*`的`I`也是可行函数，但是根据上述第3点第二条，它将被去掉。
        注：实际上是比较以下俩可行函数：

        - 从I实例化的：`int compare(const char* const&, const char* const&);`
        - II的：`int compare(const char*, const char*);`
      - 调用2：

        ```
        char ch_arr1[] = "world", ch_arr2[] = "hi";
        compare(ch_arr1, ch_arr2);
        ```
        同样也调用普通函数II。I的实例将`T`绑定到`char*`。

    - 例二：
      如果I是这样：`template<typename T> int compare(T, T);`则：
      - `compare(ch_arr1,ch_arr2);`将调用`I`，`T`绑定到`char*`。
      - `compare(const_arr2, const_arr2);`将调用II。
      - `const char *cp1 = const_arr1, *cp2 = const_arr2; compare(cp1, cp2);`将调用II。
