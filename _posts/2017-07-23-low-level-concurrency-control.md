---
layout: post
title: 底层并发控制机制
categories:
    - 系统&体系结构
tags:
    - 并发控制
    - Work in Progress
    - 学习笔记
---

{::comment} Not show line_numbers {:/comment}
{::options syntax_highlighter_opts="{default_lang: c++ \}" /}

* TOC
{:toc}

## Spin Lock

- spinlock[实现1](http://imaupk.blog.163.com/blog/static/348222512008112655221557/)，结构体类似于：
  ```
  struct spinlock {
    volatile unsigned int slock;  // Initialized to 0.
  } t;
  ```
  - 上锁：
    - 先把处理器的flag寄存器存起来，然后关中断
    - spin_lock函数：用lock前缀（锁总线？）做一个原子操作：把t.slock的最低位置1并返回原来的值。然后检查原来的值，如果是0，说明当前线程成功上锁，可以返回；否则，说明之前已被上锁，然后不断循环测试t.slock的最低位的值（忙等待），直到变为0（某其他线程释放了），再重新用lock前缀去做原子操作并重复执行上述步骤。
  - 解锁：
    - spin_unlock函数：用lock前缀将t.slock最低位置0
      > 问：这里可以不用lock前缀吗？
        {: .lambda_question}
    - 开中断，恢复flag寄存器的值
- 当我们能保证即使发生中断、在中断程序中也不会修改critical section的数据时，可以直接使用实现1的spin_lock和spin_unlock函数而无需保存flag寄存器和关中断
- 读写者spinlock：与实现1类似，只是除了用1个bit表示写者是否已经获得锁之外，用31个bit来表示当前读者的个数（这样写者很容易饿死吧？）
- spinlock[实现2](http://blog.csdn.net/yeqishi/article/details/6570208)：结构体与实现1一样，下面叙述忽略保存flag寄存器和关中断等内容
  - 上锁：用lock前缀调用decb汇编指令将t.slock的值减 1。然后用jns汇编指令检查EFLAGS寄存器的SF(符号)位，如果为 0，说明slock原来的值为1，则线程获得锁，然后结束本次函数调用；如果SF位为1，说明slock原来的值为0或负数，锁已被占用，那么线程开始不断测试t.slock与0的大小关系，直到t.slock大于0，说明锁已被释放，则跳转到开始位置重新申请锁，重复上述过程。
  - 解锁：把t.slock置为1（不用lock前缀）
- 兼顾公平：排队自旋锁
  - 排队自旋锁仍然使用原有的数据结构，但是赋予slock域新的含义。为了保存顺序信息，slock 域被分成两部分，分别保存锁持有者和未来锁申请者的票据序号(Ticket Number)：如果处理器个数不超过 256，则 Owner 域为 slock 的 0-7 位，Next 域为 slock 的 8-15 位，slock 的高 16 位不使用；如果处理器个数超过 256，则 Owner 和 Next 域均为 16 位，其中 Owner 域为 slock 的低 16 位。可见排队自旋锁最多支持 2^16=65536 个处理器。
  - 只有 Next 域与 Owner 域相等时，才表明锁处于未使用状态（此时也无人申请该锁）。排队自旋锁初始化时 slock 被置为 0，即 Owner 和 Next 置为 0。内核执行线程申请自旋锁时，原子地将 Next 域加 1，并将原值返回作为自己的票据序号。如果返回的票据序号等于申请时的 Owner 值，说明自旋锁处于未使用状态，则直接获得锁；否则，该线程忙等待检查 Owner 域是否等于自己持有的票据序号，一旦相等，则表明锁轮到自己获取。线程释放锁时，原子地将 Owner 域加 1 即可，下一个线程将会发现这一变化，从忙等待状态中退出。线程将严格地按照申请顺序依次获取排队自旋锁，从而完全解决了“不公平”问题。
- [ABA problem](https://en.wikipedia.org/wiki/ABA_problem)

## 乱序执行（out-of-order execution）和内存屏障

- 乱序执行实现了避免计算机在用于运算的对象不可获取时的大量等待。
- 乱序执行使用其他“可以执行”的指令来填补了时间的空隙，然后在在结束时重新排序运算结果来实现指令的顺序执行中的运行结果。指令在原始计算机代码中的顺序被称为程序顺序，在处理器中他们被按照数据顺序中被处理，这种顺序中，数据，运算符，在计算机寄存器中变得可以获取。一般来说乱序执行需要复杂的电路来实现转换一种顺序到另一中顺序并且维护在输出时的逻辑顺序；而处理器本身就好像是随机执行的样子。

- 内存一致性模型（[参考这里](http://www.cnblogs.com/haippy/p/3412858.html)），以下讨论的写操作的order都是指针对不同内存位置的写，对同一内存位置的写必须是顺序一致的，否则程序正确性无法保证
  - 顺序一致性（sequential consistency）模型：Lamport于1979：the result of any execution is the same as if the operations of all the processors were executed in some sequential order, and the operations of each individual processor appear in this sequence in the order specified by its program。这要求同一处理器的所有操作（包括读和写）都是顺序执行的
  - 处理器一致性（processor consistency）模型：同一处理器中，所有读操作之间是sequential consistency的，任意写操作执行前所有先于（指program order）该写操作的所有访存操作（包括读和写）都已完成。该模型允许读操作被reorder至对另一位置的写之前。
  - 弱一致性（weak consistency）模型：把普通的读写访存操作和同步操作进行区分。所有同步操作之间是sequential consistency的，任一同步操作开始前所有先于该操作的普通访存操作都已完成，任一普通访存操作开始前所有先于该操作的同步操作都已完成。
  - 释放一致性（release consistency）模型：把同步操作进一步分为acquire和release操作。所有同步操作之间是sequential consistency的，任一release操作开始前所有先于该操作的普通访存操作都已完成，任一普通访存操作开始前所有先于该操作的acquire操作都已完成。
- x86-64的内存模型：
  - Loads are not reordered with other loads.
  - Stores are not reordered with other stores.
  - Stores are not reordered with older loads.
  - <span style="color: blue">Loads may be reordered with older writes to different locations but not with older writes to the same location.</span>
  - In a multiprocessor system, memory ordering obeys causality （因果关系） (memory ordering respects transitive visibility).
  - In a multiprocessor system, stores to the same location have a total order.
  - In a multiprocessor system, locked instructions have a total order.
  - Loads and stores are not reordered with locked instructions.
- `asm volatile("" ::: "memory");` creates a <span style="color: blue">compiler level</span> memory barrier forcing <span style="color: blue">optimizer</span> to not re-order memory accesses across the barrier.
- 指令流水线（pipelining）：把一条指令分为取址、解码、执行、存储结果、结束等几个部分，连续的指令的不同部分并行执行，形成一个流水线
- lfence/sfence/mfence from intel manual section 8.2.2
  - Reads cannot pass earlier LFENCE and MFENCE instructions.
  - Writes cannot pass earlier LFENCE, SFENCE, and MFENCE instructions.
  - LFENCE instructions cannot pass earlier reads.
  - SFENCE instructions cannot pass earlier writes.
  - MFENCE instructions cannot pass earlier reads or writes.
  - by i and iii, lfence enforces #LoadLoad ordering
  - by ii and iii, lfence enforces #LoadStore ordering
  - by ii and iv, sfence enforces #StoreStore ordering
  - sfence not enforces #StoreLoad ordering
- lock 前缀的访存指令相当于 Full Barrier，xchg 已隐式加 lock 前缀。
- 具有acquire语义的内存屏障，保证其后的读写不会提前到屏障之前
- 具有release语义的屏障，保证之前的读写已经生效/可见
- acquire和release这两种内存屏障都是单向的，即允许临界区外的读写进入到临界区内。具体到 X86-64 架构，考虑前面描述的内存模型，读操作就具有acquire语义（因为一个读操作其后的读写都不会被reorder到该读操作之前），而写操作具有release语义（因为一个写操作之前的读写都不会被reorder到该写操作之后）
- [MSEI protocol](https://en.wikipedia.org/wiki/MESI_protocol)

## [原子操作和volatile关键字](http://baiy.cn/doc/cpp/advanced_topic_about_multicore_and_threading.htm#%E5%8E%9F%E5%AD%90%E6%93%8D%E4%BD%9C%E5%92%8C_volatile_%E5%85%B3%E9%94%AE%E5%AD%97)

- C/C++ 中的volatile关键字提供了以下保证：
  - 对声明为volatile的变量进行的任何操作都不会被优化器去除，即使它看起来没有意义（例如：连续多次对某个变量赋相同的值），因为它可能被某个在编译时未知的外部设备或线程访问。
  - 被声明为volatile的变量不会被编译器优化到寄存器中，每次读写操作都保证在内存(详见下文)中完成。
  - 在不同表达式内的多个volatile变量间的操作顺序不会被优化器调换（即：编译器保证多个volatile变量在 sequence point之间的访问顺序不会被优化和调整）。
- volatile<span style="color: blue">**不**</span>提供如下保证
  - volatile声明不保证读写和运算操作的原子性。
  - volatile声明不保证对其进行的读写操作直接发生在主内存。相反，CPU 会尽可能让这些读写操作发生在 L1/L2 等cache上。除非：
    - 发生了一个未命中的读请求。
    - 所有级别的cache均已被配置为通过式写（write through）。
    - 目标地址为 non-cacheable区（主要是其它设备映射到内存地址空间的通信接口。例如：网卡的板载缓冲区、显卡板载显存、WatchDog寄存器等等）。
  - 编译器仅保证在生成目标码时不调整volatile变量的访问顺序，但通常并不保证该变量不受处理器的out-of-order特性影响。目前唯一一个已知的特例是安腾（IA64）处理器版的 VC：在生成 IA64 Target时，VC会自动在所有volatile访问之间添加内存屏障（详见下文）以保证访问顺序。但ISO标准并未要求编译器实现类似机制。实际上，其它编译器（或是面向其它平台的 VC）也都没有类似保证。也就是说，通常认为volatile并不保证代码在处理器上的执行顺序，如果需要类似的保证，程序员应当自己使用内存屏障操作。
- 而原子操作要求做到以下保证：
  - 对原子量的 '读出-计算-写入' 操作序列是原子的，在以上动作完成之前，任何其它处理器和线程均无法访问该原子量。
  - 原子操作必须保证缓存一致性。即：在多处理器环境中，原子操作要同时保持所有处理器上的各级cache之间、以及它们与主存间的访问一致性。
- 可见，使用volitale关键字并不足以保证操作的原子语义。volitale关键字的主要设计目的是支持C/C++程序与内存映射设备间的通信。但这并不是说volitale关键字对原子操作没有任何帮助：
  - 对于一个被声明为volitale类型的原子量来说，如果所有写操作都是原子的，并且已提供了必要的cache一致性保证，那么读取该原子量时就不必再次对总线上锁。
  - 这是因为volitale关键字保证了对该变量的读操作起码是在当前 CPUcache中完成的（即：该变量不会被优化到寄存器中）。与此同时，只要当前环境能够保证多个处理器的cache之间，以及cache与主存之间的访问一致性。那么该读操作不管在主存还是当前系统中任意一个处理器的cache上发生，读到的都是一致的数据。
  - 在这里，volitale关键字配合原子量的写入操作一起实现了一个典型的读者/写者同步模型：所有写操作和cache同步都保证被原子、互斥地完成；读操作则可以被并发地实现。这也是 Windows API 中没有提供类似 'AtomicLoad' 式语义操作的原因。
  - 当然，这里还有一个隐含的附加条件，就是 CPU 必须能够在一个操作中读入这个原子量。这个条件通常可以忽略，因为超出CPU位宽的数据类型通常无法被实现成真正的原子量类型。
