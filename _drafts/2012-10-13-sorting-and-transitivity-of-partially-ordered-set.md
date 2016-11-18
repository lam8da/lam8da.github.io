---
layout: post
title: 排序与偏序关系的传递性
categories:
    - 算法&数学
tags:
    - Work in Progress
    - 排序
    - 数理逻辑
    - 算法
---

{::comment} Not show line_numbers {:/comment}
{::options syntax_highlighter_opts="{default_lang: c++ \}" /}

1. TOC
{:toc}

Given N strings s1, s2, ..., sn, sort them in some way, then connect them one by
one, we obtain a long string. Now the task is to find an arrangement of the
strings such that the long string we obtain is lexicographically least among all
possible long strings.

There is a simple solution to this problem. For any two strings A and B, we
define that A is prior to B, if and only if A+B<B+A (here the symbol '+' means
connection, '<' means 'lexicographically less than'). Then we can use any of the
sorting algorithms to sort the strings by their priority and obtain the
arrangement required.

But there is a problem: base on the comparison rule defined above, how to prove
that the strings can be sorted correctly? That is, how to prove that the strings
are sortable by the rule?

Clearly, any two strings are comparable under the rule. So the problem is
whether the rule would produce a circle. If there is a circle, we failed;
otherwise we can sort them in topological order.

Suppose that the rule is transitive, that is to say, if A is prior to B and B is
prior to C, then A must be prior to C. If the transitivity holds, there will be
no circles, and the strings are sortable under the rule.

Let la, lb and lc denote the length of string A, B and C respectively. We know
that any string S can be expressed by a number of radix Z, where Z can be the
size of the alphabet (e.g. 26). Let Val(S) denote the number representing string
S, then string A+B can be expressed as:

Val(A+B)=Val(A)*Z^lb+Val(B)

Similarly,

Val(B+A)=Val(B)*Z^la+Val(A)

Val(B+C)=Val(B)*Z^lc+Val(C)

Val(C+B)=Val(C)*Z^lb+Val(B)

Val(A+C)=Val(A)*Z^lc+Val(C)

Val(C+A)=Val(C)*Z^la+Val(A)

Since Length(A+B) is equal to Length(B+A), the relation A+B<B+A is equivalent
to the following expression in algebra:

Val(A)*Z^lb+Val(B)<Val(B)*Z^la+Val(A)　　　　······　　　　*

Similarly, B+C<C+B is equivalent to:

Val(B)*Z^lc+Val(C)<Val(C)*Z^lb+Val(B)　　　　······　　　　#

From (*) we can obtain:

Val(A)*(Z^lb-1)<Val(B)*(Z^la-1)

Recognizing that la>0 and Z>1, we get Z^la>=Z^1>1, so Z^la-1>0. Then we
obtain:

Val(A)*(Z^lb-1)/(Z^la-1)<Val(B)　　　　······　　　　$

Similarly, we can obtain from (#) that:

Val(B)<Val(C)*(Z^lb-1)/(Z^lc-1)　　　　······　　　　$$

At last, we get from ($) and ($$) that:

Val(A)*(Z^lb-1)/(Z^la-1)<Val(C)*(Z^lb-1)/(Z^lc-1)

Then we can easily obtain: Val(A)*Z^lc+Val(C)<Val(C)*Z^la+Val(A), which is
equivalent to A+C<C+A.

As proved above, the strings can be sorted under the rule. And after
sorting, according to the quality of sorting algorithms, we can prove
that the first string in the arrangement is prior to any others, so it
makes the long string lexicographically less than any long strings
starting with other strings. For the second and other strings in the
arrangement, we can prove the optimality similarly.

有n个元素，每个元素是一个二元组<ai,bi>，对这些元素进行排列，使得max{a0*a1*a2*...*a(i-1)/bi:0<=i<n}最小。

解：按照ai*bi的大小进行排序即可。
