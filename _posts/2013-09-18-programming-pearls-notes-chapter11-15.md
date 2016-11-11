---
layout: post
title: 《编程珠玑》读书笔记（第11-15章完）
categories:
    - 算法&数学
tags:
    - 算法
    - 读书笔记
---

* TOC
{:toc}

## 第11章 排序

使用快排思想在O(字符总数)的复杂度下对n个长度不一的01串排序：设x[0,…,n-1]中每一项都包含一个整数length和一个指向数组bit[1,…,length]的指针：

{% highlight c linenos %}
void bsort(l, u, depth)
  if l >= u return
  for i = [l, u]
    if x[i].length < depth
      swap(i, l++)
  m = l
  for i = [l, u]
    if x[i].bit[depth] == 0
      swap(i, m++)
  bsort(l, m-1, depth+1)
  bsort(m, u, depth+1)
{% endhighlight %}

这其实就是一个基数排序思想。

## 第12章 取样问题

### 输出0~n-1范围内的m个随机整数（不能重复），要求每个子集被选取的可能性相等（这比要求每个数均以m/n概率选中的要求更强）。

解：Knuth的方法，时间复杂度O(n)，空间复杂度O(1)：

{% highlight c linenos %}
select = m
remaining = n
for i = [0,n)
  if (bigrand() % remaining) < select
    print i
    select--
  remaining--
{% endhighlight %}

证明：设选择的数为i1,i2,i3,…,im。则选中这组数的概率为：
P=[1-m/n][1-m/(n-1)]*…*[1-m/(n-(i1-1))]*[m/(n-i1)]*
[1-(m-1)/(n-i1-1)][1-(m-1)/(n-i1-2)]*…*[1-(m-1)/(n-(i2-1))]*[(m-1)/(n-i2)]*
…*
[1-1/(n-i{m-1}-1)][1-1/(n-i{m-1}-2)]*…*[1-1/(n-(im-1))]*[1/(n-im)]
=m!(n-m)!/n!=1/C(n, m)。故。

### 上一题中Floyd的基于集合的随机选择算法，用set实现，时间复杂度O(mlogm)，空间复杂度O(m)：

{% highlight c linenos %}
set S;
for(int j=n-m+1; j < n; j++) {
  int t = bigrand() % j;
  if(S.find(t) == S.end()){
    S.insert(t); // t not in S
  } else {
    S.insert(j); // t in S
  }
}
{% endhighlight %}

证明：首先，前n-m+1个数（即0到n-m）的每一个最终不被选中的概率都是（第一轮不被选中，且，第二轮不被选中，且，……，故用乘法）： (n-m)/(n-m+1)*(n-m+1)/(n-m+2)*...*(n-2)/(n-1)*(n-1)/n=(n-m)/n，故用1减即得。 对于数i(i>n-m)，由于其前x=i-(n-m)轮都没有机会被选中，而从第x+1轮（此前已经选了i-(n-m)个数）开始，最终不被选中的概率是：
第x+1轮不被选中的概率*以后每一轮都不被选中的概率
(1-(i-(n-m)+1)/(i+1))*(i+1)/(i+2)*…*(n-1)/n=(n-m)/n.
问：如何证明每个子集被选中的概率都相等？

### 编程过程中几个重要步骤
1. 正确理解所遇到的问题
1. 提炼出抽象问题
1. 考虑尽可能多的解法
1. 实现一种解决方案

### 习题2：从n个元素中随机选m个，给出一个算法，其中每个元素的选中概率相等，但某些子集的选中概率比其他子集大。
解：只随机一次，得出一个数i，则选择i, (i+1)mod n, (i+2)mod n, …, (i+m-1)mod n。

## 第14章 堆

1. 习题5：装箱问题：将n个权值（每个都介于0和1之间）分配给最少数目的单位容量箱。
其启发式解法：将每个权值放到第一个合适的箱中（按升序扫描箱）。实现方式如下：
先考虑线段树解法：对箱数组建立线段树，每条线段表示该线段包含的箱子的最大剩余容量，即可将升序扫描的复杂度降为O(logn)。
再考虑简化：可直接构建一棵类似堆的二叉树而不用另外开辟O(n)的空间构建线段树。

2. 给一个单链表的每个结点额外增加一个指针，使得访问第i个元素的时间为O(logn)。
解：让每个结点i指向结点2i，则访问路径为：

View Code C
1
2
3
4
5
6
7
8
void path(n)
  if n == 0 print "start at 0, "
  else if even(n)
    path(n/2)
    print "double to ", n
  else
    path(n-1)
    print "increment to ", n
3. 另外一种二分搜索结构：把一个2^k-1元有序数组拷贝到一个“堆搜索”数组b中：a中奇数位元素按顺序放到b的后半部分，模4余2位置的元素按顺序放到b的剩余部分的后半部分，etc。修改后的二分搜索从i=1开始，每次将i置为2i或2i+1。
本质：这其实是一颗静态二叉排序树。
