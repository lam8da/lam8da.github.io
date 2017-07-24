---
layout: post
title: 互斥算法
categories:
    - 算法&数学
tags:
    - 并发控制
    - 互斥算法
    - Work in Progress
    - 学习笔记
---

{::comment} Not show line_numbers {:/comment}
{::options syntax_highlighter_opts="{default_lang: c++ \}" /}

* TOC
{:toc}

## Peterson算法（要注意CPU对内存访问顺序的优化改变）

- Initialization
  ```
  flag[0] = flag[1] = false;
  turn = 0;
  ```
- P0:
  ```
  flag[0] = true;
  turn = 0;
  while (flag[1] == true && turn == 0);
  ...  /* critical section here */
  flag[0] = false;
  ```
- P1:
  ```
  flag[1] = true;
  turn = 1;
  while (flag[0] == true && turn == 1);
  ...  /* critical section here */
  flag[1] = false;
  ```

## Filter算法：N大于2时的Peterson算法

- Initialization
  ```
  level[0, ..., N-1] = { -1 };  // current level of processes 0...N-1

  // the waiting process of each level 0...N-2.
  // 这是一个排队队列，任何时刻当所有进程在以下while忙等待稳定后或在临界区执行中
  // 时（假设临界区足够长），不可能有两个或以上pending进程的level为同一个值
  waiting[0, ..., N-2] = { -1 };
  ```
- Code for process #i
  ```
  for(l = 0; l < N-1; ++l) 
    level[i] = l;
    waiting[l] = i;
    while(waiting[l] == i && (there exists k != i, such that level[k] >= l));  // busy wait
  }
  ...  /* critical section here */
  level[i] = 0;  // exit section
  ```

## Deker算法

- Initialization
  ```
  flag[0] = f[1] = false;
  turn = 0;  // or 1. The init value determines the first
             // process to enter the critical section
  ```

- P0:
  ```
  flag[0] = true;
  while (flag[1] == true) {
    if (turn != 0) {
      flag[0] = false;
      while (turn != 0);  // busy wait
      flag[0] = true;
    }
  }
  ...  /* critical section here */
  turn = 1;
  flag[0] = false;
  ```
- P1:
  ```
  flag[1] = true;
  while (flag[0] == true) {
    if (turn != 1) {
      flag[1] = false;
      while (turn != 1);  // busy wait
      flag[1] = true;
    }
  }
  ...  /* critical section here */
  turn = 0;
  flag[1] = false;
  ```

## Lamport面包店算法

```
// Declaration and initial values of global variables
Entering: array [1..NUM_THREADS] of bool = {false};
Number: array [1..NUM_THREADS] of integer = {0};

lock(integer i) {
  Entering[i] = true;
  Number[i] = 1 + max(Number[1], ..., Number[NUM_THREADS]);
  Entering[i] = false;
  for (j = 1; j <= NUM_THREADS; j++) {
    // Wait until thread j receives its number:
    while (Entering[j]);
    // 上述这个while必不可少，否则在下面的<判断中可能会有两个（或多个？）线程
    // 同时失效而同时进入critical section：考虑两个线程都把Number设为同一值但是
    // j<i且i先到达此处的情况，这样j可能还未设置Number[j]，于是i将读到0，下面的
    // while失效而进入临界区；而当j进入时，下面的小于号判断失效，也进入临界区

    // Wait until all threads with smaller numbers or with the same number,
    // but with higher priority, finish their work:
    while ((Number[j] != 0) && ((Number[j], j) < (Number[i], i)));
  }
}
unlock(integer i) {
  Number[i] = 0;
}
Thread(integer i) {
  while (true) {
    lock(i);
    ...  /* critical section here */
    unlock(i);
    ...  // non-critical section
  }
}
```

## Eisenberg & McGuire algorithm

```
enum pstate = {IDLE, WAITING, ACTIVE};
pstate flags[n];
int turn;
// The variable turn, is set arbitrarily to a number between 0 and n-1 at the
// start of the algorithm.
// The flags variable, for each process is set to WAITING whenever it intends to
// enter the critical section.
// flags takes either IDLE or WAITING or ACTIVE.
// Initially the flags variable for each process is initialized to IDLE.
repeat {
  flags[i] := WAITING;  /* announce that we need the resource */

  /* scan processes from the one with the turn up to ourselves. */
  /* repeat if necessary until the scan finds all processes idle */
  index := turn;
  while (index != i) {
    if (flags[index] != IDLE) index := turn;
    else index := (index+1) mod n;
  }

  flags[i] := ACTIVE;  /* now tentatively claim the resource */

  /* find the first active process besides ourselves, if any */
  index := 0;
  while ((index < n) && ((index = i) || (flags[index] != ACTIVE))) {
    index := index+1;
  }

  // if there were no other active processes, AND if we have the turn or else
  // whoever has it is idle, then proceed.
  // Otherwise, repeat the whole sequence.
} until ((index >= n) && ((turn = i) || (flags[turn] = IDLE)));

turn := i;  /* claim the turn and proceed */

/* Critical Section Code of the Process */

// find a process which is not IDLE (if there are no others, we will find
// ourselves)
index := turn+1 mod n;
while (flags[index] = IDLE) {
  index := (index+1) mod n;
}

turn := index;  /* give the turn to someone that needs it, or keep it */
flags[i] := IDLE;  /* we're finished now */
```

## Szymanski算法

```
// 进入临界区的协议：
flag[self] = 1  // 站在入口门外，申请进入等候室
// 等待入口门打开。即不存在有进程处于3、4状态，包括正在进门、正在使用临界区
await_until(all flag[1..N] in {0, 1, 2})
flag[self] = 3  // 站在入口门处，即正在进入。
if any flag[1..N] = 1:  // 如果还有在入口门外等待进入的线程
  flag[self] = 2  // 把自己置为在入口门内，等待所有提出申请的线程都完成进入等候室
  await_until(any flag[1..N] = 4)  // 等待最后进门的线程关闭入口门
flag[self] = 4  // 门处于关闭状态，线程在等候室
await_until(all flag[1..self-1] in {0, 1})
// 等待所有具有更高优先级的线程访问完临界区，退出等候室

// 这里是临界区...

// 退出临界区的协议
// 确保所有比自己优先级低的已经通过入口门的线程都进入了等候室
await_until(all flag[self+1..N] in {0, 1, 4})
flag[self] = 0  // 离开等候室，如果自己是最后离开的，则入口门被打开
```
