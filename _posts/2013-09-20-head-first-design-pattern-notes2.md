---
layout: post
title:  《Head First设计模式》读书笔记（下）- 设计模式
categories:
    - 程序&设计
tags:
    - 设计模式
    - 读书笔记
---

1. TOC
{:toc}

{::comment} Not show line_numbers {:/comment}
{::options syntax_highlighter_opts="{default_lang: java \}" /}

## 策略模式

策略模式定义了算法族，分别封装起来，让它们之间可以互相替换，此模式让算法的变化独立于使用算法的客户。

![strategy]({{ site.url }}/assets/2013-09-20-head-first-design-pattern-notes2.strategy.png)

## 观察者模式

观察者模式定义了对象之间的一对多依赖，这样一来，当一个对象改变状态时，它的所有依赖者都会收到通知并自动更新。以下是Java的观察者模式：

![observers]({{ site.url }}/assets/2013-09-20-head-first-design-pattern-notes2.observers.png)

1. 出版者+订阅者=观察者模式
1. 不要依赖于观察者被通知的次序，因为一旦观察者/可观察者的实现有所改变，通知次序就会改变，很可能就会产生错误的结果，这绝对不是我们认为的松耦合
1. `java.util.Observale`的黑暗面：`Observable`是一个类，如果某类想同时具有`Observable`类和另一个超类的行为，就会陷入两难，毕竟Java不支持多重继承，这限制了`Observable`的复用潜力；另外，`Observable`将关键方法（`setChanged()`）保护起来，这个设计违反了设计原则：多用组合，少用继承。

## 装饰者模式

装饰者模式动态地将责任附加到对象上。若要扩展功能，装饰者提供了比继承更有弹性的替代方案。

![decorator]({{ site.url }}/assets/2013-09-20-head-first-design-pattern-notes2.decorator.png)

1. 如上述类图关系，重点在于，装饰者和被装饰者必须是一样的类型，也就是有同样的超类，这相当关键。在这里，我们利用继承达到“类型匹配”而不是获得“行为”。
1. 为何装饰者和被装饰者有相同的接口：因为装饰者必须能取代被装饰者。
1. 通常装饰者模式是采用抽象类，但是在Java中可以使用接口

## 工厂方法模式（Factory Method Pattern）

工厂方法模式定义了一个创建对象的接口，但由子类决定要实例化的类是哪一个。工厂方法让类把实例化推迟到子类，通过让子类决定该创建的对象是什么，来达到将对象创建的过程封装的目的。

工厂模式具有平行的类层级：

![factory]({{ site.url }}/assets/2013-09-20-head-first-design-pattern-notes2.factory.png)

1. 当可能有许多客户程序通过各种方法来创建一个类型的新对象时，可以考虑到把创建这个对象的方法放到一个工厂中去，即使用工厂模式。所有工厂模式都用来封装对象的创建。
1. 工厂方法用来处理对象的创建，并将这样的行为封装在子类中。这样，客户程序中关于超类的代码就和子类对象创建代码解耦了。工厂方法不一定要真的放到一个非本类的“工厂类”中。
1. 使用“工厂”意味着客户在实例化对象时，只会依赖于接口而不是具体类。这符合依赖倒置原则。

## 抽象工厂模式

抽象工厂模式提供一个接口，用于创建相关或依赖对象的家族，而不需要明确指定具体类。

![abstract-factory]({{ site.url }}/assets/2013-09-20-head-first-design-pattern-notes2.abstract-factory.png)

1. 抽象工厂的方法经常以工厂方法的方式实现。抽象工厂的任务是定义一个负责创建一组产品的接口，这个接口内的每个方法都负责创建一个具体产品，同时我们利用实现抽象工厂的子类来提供这些具体的做法
1. 抽象工厂和工厂方法都是负责创建对象，工厂方法用的方法是继承，抽象工厂是通过对象的组合
1. 当需要创建产品家族和想让制造的相关产品集合起来时，可以使用抽象工厂；而工厂方法可以把客户代码从需要实例化的具体类中解耦，或如果目前还不知道将来需要实例化哪些具体类时，也可以用工厂方法。
1. 所有工厂模式都通过减少应用程序和具体类之间的依赖促进松耦合

## 单件模式（Singleton Pattern）

单件模式用来创建独一无二的、只能有一个实例的对象。

1. 实现方法：若将对象赋值给一个全局变量，那么必须在程序一开始就创建好对象。万一这个对象非常耗资源，而程序在这次执行过程中又一直没用到它，就会形成浪费。
1. 单件模式实现中处理多线程问题的方法：
   ```java
   public class Singleton {
     private volatile static Singleton uniqueInstance;
     private Singleton() {
     }
     public static Singleton getInstance() {
       if (uniqueInstance == null) {
         synchronized(Singleton.class) {
           if (uniqueInstance == null) {
             uniqueInstance = new Singleton();
           }
         }
       }
       return uniqueInstance;
     }
   }
   ```
   使用`synchronized`关键字可能会导致效率严重下降。而volatile关键字定义的成员变量确保在同一时间最多只有一个线程对其进行访问，尤其是32位系统处理64位变量时。另外，也可以使用“急切”创建实例而不用延迟实例，在静态初始化器（static initializer）中创建单位：
   ```
   private static Singleton uniqueInstance = new Singleton();
   ```
   但这样会导致1中提到的问题。

## 命令模式（封装调用）

将“请求”封装成对象，以便使用不同的请求、队列或者日志来参数化其他对象。命令模式也支持可撤销的操作。

![command]({{ site.url }}/assets/2013-09-20-head-first-design-pattern-notes2.command.png)

1. 当需要将发出请求的对象和执行请求的对象解耦的时候，使用命令模式
1. 宏命令模式：

![macro-command]({{ site.url }}/assets/2013-09-20-head-first-design-pattern-notes2.macro-command.png)

## 适配器模式

适配器模式将一个类的接口，转换成客户期望的另一个接口。适配器让原本接口不兼容的类可以合作无间。

![adapter]({{ site.url }}/assets/2013-09-20-head-first-design-pattern-notes2.adapter.png)

## 外观模式

外观模式改变接口，将一个或数个类的复杂的一切都隐藏在背后，只显露出一个干净美好的外观，让接口更简单。

外观模式提供了一个统一的接口，用来访问子系统中的一群接口。外观定义了一个高层接口，让子系统更容易使用。

## 模板方法模式

模板方法模式在一个方法中定义一个算法的骨架，而将一些步骤延迟到子类中。模板方法使得子类可以在不改变算法结构的情况下，重新定义算法中的某些步骤。

1. 模板方法不一定要使用继承，例如c++的stl的排序方法使用的比较函数
1. 常见形式：
   - 抽象方法：当子类必须提供算法中某个步骤的实现时使用
   - 钩子：当算法某个部分可选时使用
1. 为了防止子类改变模板方法中的算法，可以将模板方法声明为`final`

## 迭代器模式

迭代器模式提供一种方法顺序访问一个聚合对象中的各个元素，而又不暴露其内部的表示

![iterator]({{ site.url }}/assets/2013-09-20-head-first-design-pattern-notes2.iterator.png)

## 组合模式

组合模式允许你将对象组合成树形结构来表现“整体/部分”层次结构。组合能让客户以一致的方式处理个别对象以及对象组合

![composite]({{ site.url }}/assets/2013-09-20-head-first-design-pattern-notes2.composite.png)

组合模式是让一个类有两个责任的模式，它不但要管理层次结构，而且还要执行菜单的操作。但组合模式以单一责任设计原则换取透明性（transparency）：通过让组件的接口同事包含一些管理子节点和叶节点的操作，客户就可以将组合和叶节点一视同仁，这是一个很典型的折中案例。

## 状态模式

允许对象在内部状态改变时改变它的行为，对象看起来好像修改了它的类。

![state]({{ site.url }}/assets/2013-09-20-head-first-design-pattern-notes2.state.png)

上述`ConcreteStateA`和`ConcreteStateB`对`Context`的引用并不是必须的。有时`Context`本身也可以决定状态转换的流向。当状态转换是固定时（例如做了某个动作后就一定转换到某特定状态），就适合放在`Context`中。当转换更动态的时候，通常就会放在状态类中。做这个决策的同时，也等于是在为另一件事情做决策：当系统进化时，究竟哪个类是对修改封闭。

多个`Context`实例可共享状态对象（使用静态变量）。若状态需要利用`Context`中的方法，还必须在每个`handler()`方法内传入一个`Context`引用。

## 代理模式

代理模式为另一个对象提供一个替身或占位符以控制对这个对象的访问。

![proxy]({{ site.url }}/assets/2013-09-20-head-first-design-pattern-notes2.proxy.png)

1. 几种代理模式：
   - 远程代理：控制访问远程对象
   - 虚拟代理：控制访问创建开销大的资源
   - 保护代理：基于权限控制对资源的访问
   - 防火墙代理（Firewall Proxy）
   - 智能引用代理（Smart Reference Proxy）
   - 缓存代理（Caching Proxy）
   - 同步代理（Synchronization Proxy）
   - 复杂隐藏代理（Complexity Hiding Proxy）
   - 写入时复制代理（Copy-On-Write Proxy）
1. Java RMI（远程过程调用）
   - java.rmi.Remote
   - stub、skeleton
   - rmic、rmiregistry工具
   - transient关键字：告诉Java不要序列化一个对象
1. Java的动态代理技术：

![java-dynamic-proxy]({{ site.url }}/assets/2013-09-20-head-first-design-pattern-notes2.java-dynamic-proxy.png)

## MVC：模型-视图-控制器

![mvc]({{ site.url }}/assets/2013-09-20-head-first-design-pattern-notes2.mvc.png)

图解：

1. 策略模式：视图是一个对象，可以被调整使用不同的策略，而控制器提供了策略
1. 视图用组合模式组织各窗口、面板、按钮等
1. 模型实现了观察者模式，当状态改变时通知对象更新。这使得同一模型可以使用不同的视图甚至可使用多个视图
1. 不同接口的模型常常通过适配器模式复用同一个视图

![mvc-web]({{ site.url }}/assets/2013-09-20-head-first-design-pattern-notes2.mvc-web.png)

上图是Web中的MVC，称为Model 2（Servlet+JSP）。

复合模式：结合两个或以上的模式，组成一个解决方案，解决一再发生的一般性问题。

## 模式分类

模式是在某情景下，针对某问题的某种解决方案。模式可分为以下几类：

- 创建型：涉及到将对象实例化，这类模式都提供一个方法，将客户从所需要实例化的对象中解耦。包括：Singleton、Builder、Prototype、Abstract Factory、Factory Method
- 行为型：设计到类和对象如何交互及分配的职责。包括：Template Method、Visitor、Mediator、Iterator、Command、Memento、Interpreter、Observer、Chain of Responsibility、State、Strategy
- 结构型：可让你把类或对象组合到更大的结构中。包括：Decorator、Proxy、Composite、Façade、Flyweight、Bridge、Adapter

反模式告诉你如何采用一个不好的解决方案解决一个问题，告诉你为何这个方案从长远看会造成不好的影响，警告你不要陷入某种致命的诱惑，建议你改用其他的模式以提供更好的解决方案。
