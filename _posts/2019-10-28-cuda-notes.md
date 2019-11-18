---
layout: post
title: CUDA程序设计的一些笔记
date: 2019-10-28 21:12:23.665137
categories:
    - 系统&体系结构
tags:
    - CUDA
    - 高性能计算
    - Work in Progress
---

{::comment} Not show line_numbers {:/comment}
{::options syntax_highlighter_opts="{default_lang: c++ \}" /}

* TOC
{:toc}

# CUDA C Best Practices Guide

## 9.1.2: Asynchronous and Overlapping Transfers with Computation

异步数据复制cudaMemcpyAsync要求pinned host memory（really？？）

## 9.1.3: Zero Copy

CUDA kernel可以直接访问mapped pinned (non-pageable) memory，具体操作如下：

```
float *a_h, *a_map;
...
cudaGetDeviceProperties(&prop, 0);
if (!prop.canMapHostMemory) exit(0);
cudaSetDeviceFlags(cudaDeviceMapHost);
cudaHostAlloc(&a_h, nBytes, cudaHostAllocMapped);
cudaHostGetDevicePointer(&a_map, a_h, 0);
kernel<<<gridSize, blockSize>>>(a_map);
```

由于数据不会cache在GPU中，因此mapped pinned memory should be read or written only once。

## 9.2.1: Coalesced Access to Global Memory

L1 cache line is 128B; L2 cache line is 32B。

## 9.2.2: Shared Memory

同一个warp的多个线程访问同一shared memory的地址是不会导致bank conflict的，而是broadcast

使用shared memory的优点：

- to enable coalesced accesses to global memory, especially to avoid large strides (for general matrices, strides are much larger than 32)
- to eliminate (or reduce) redundante loads from global memory
- to avoid wasted bandwidth

## 9.2.4: Texture Memory

The read-only texture memory space is cached. Therefore, a texture fetch costs one device memory read only on a cache miss; otherwise, it just costs one read from the texture cache. The texture cache is optimized for 2D spatial locality, so threads of the same warp that read texture addresses that are close together will achieve best performance. Texture memory is also designed for streaming fetches with a constant latency; that is, a cache hit reduces DRAM bandwidth demand, but not fetch latency.

## 9.2.5: Constant Memory

There is a total of 64 KB constant memory on a device. The constant memory space is cached. As a result, a read from constant memory costs one memory read from device memory only on a cache miss; otherwise, it just costs one read from the constant cache. Accesses to different addresses by threads within a warp are serialized, thus the cost scales linearly with the number of unique addresses read by all threads within a warp. As such, the constant cache is best when threads in the same warp accesses only a few distinct locations. If all threads of a warp access the same location, then constant memory can be as fast as a register access.

## 9.2.6: Registers

The latency of read-after-write dependencies is approximately 24 cycles, but this latency is completely hidden on multiprocessors that have sufficient warps of threads concurrent per multiprocessor (as many as 768 threads (24 warps) might be required to completely hide latency).

## 10.1.1: Calculating Occupancy

the maximum number of registers per thread can be set manually at compilation time per-file using the `-maxrregcount` option or per-kernel using the `__launch_bounds__` qualifier

## 10.3: Multiple Contexts

While multiple contexts (and their associated resources such as global memory allocations) can be allocated concurrently on a given GPU, only one of these contexts can execute work at any given moment on that GPU; contexts sharing the same GPU are time-sliced. Creating additional contexts incurs memory overhead for per-context data and time overhead for context switching. Furthermore, the need for context switching can reduce utilization when work from several contexts could otherwise execute concurrently (see also Concurrent Kernel Execution).

Therefore, it is best to avoid multiple contexts per GPU within the same CUDA application. To assist with this, the CUDA Driver API provides methods to access and manage a special context on each GPU called the primary context. These are the same contexts used implicitly by the CUDA Runtime when there is not already a current context for a thread.

```
// When initializing the program/library
CUcontext ctx;
cuDevicePrimaryCtxRetain(&ctx, dev);

// When the program/library launches work
cuCtxPushCurrent(ctx);
kernel<<<...>>>(...);
cuCtxPopCurrent(&ctx);

// When the program/library is finished with the context
cuDevicePrimaryCtxRelease(dev);
```

## 10.5: Thread and Block Heuristics

Rules of thumb:

- Threads per block should be a multiple of warp size to avoid wasting computation on under-populated warps and to facilitate coalescing.
- A minimum of 64 threads per block should be used, and only if there are multiple concurrent blocks per multiprocessor.
- Between 128 and 256 threads per block is a better choice and a good initial range for experimentation with different block sizes.
- Use several (3 to 4) smaller thread blocks rather than one large thread block per multiprocessor if latency affects performance. This is particularly beneficial to kernels that frequently call `__syncthreads()`.

## 11.1.4: Exponentiation With Small Fractional Arguments

For some fractional exponents, exponentiation can be accelerated significantly compared to the use of pow() by using square roots, cube roots, and their inverses. For those exponentiations where the exponent is not exactly representable as a floating-point number, such as 1/3, this can also provide much more accurate results, as use of pow() magnifies the initial representational error.

## 11.2: Memory Instructions

When accessing uncached local or global memory, there are 400 to 600 clock cycles of memory latency.

## 12.2: Branch Predication

The compiler replaces a branch instruction with predicated instructions only if the number of instructions controlled by the branch condition is less than or equal to a certain threshold: If the compiler determines that the condition is likely to produce many divergent warps, this threshold is 7; otherwise it is 4.

## 12.3: Loop Counters Signed vs. Unsigned

In the C language standard, unsigned integer overflow semantics are well defined, whereas signed integer overflow causes undefined results. Therefore, the compiler can optimize more aggressively with signed arithmetic than it can with unsigned arithmetic. For example:

```
for (i = 0; i < n; i++) {
  out[i] = in[offset + stride*i];
}
```

Here, the sub-expression `stride*i` could overflow a 32-bit integer, so if `i` is declared as unsigned, the overflow semantics prevent the compiler from using some optimizations that might otherwise have applied, such as strength reduction. If instead `i` is declared as signed, where the overflow semantics are undefined, the compiler has more leeway to use these optimizations.

## 12.4: Synchronizing Divergent Threads in a Loop

Synchronizing threads inside potentially divergent code (e.g., a loop over an input array) can cause unanticipated errors. Care must be taken to ensure that all threads are converged at the point where `__syncthreads()` is called. For example:

```
unsigned int imax = blockDim.x * ((nelements + blockDim.x - 1)/ blockDim.x);
for (int i = threadidx.x; i < imax; i += blockDim.x) {
  if (i < nelements) {
    ...
  }
  __syncthreads();
  if (i < nelements) {
    ...
  }
}
```

# Profiling

- [Memory Transactions](https://docs.nvidia.com/gameworks/content/developertools/desktop/analysis/report/cudaexperiments/sourcelevel/memorytransactions.htm)
- https://docs.nvidia.com/gameworks/index.html

# 个人经验&总结

- 不要过早优化！
- CUDA的并行模型是分层次的，从fine grain到coarse grain包括：
  - thread level
  - warp level
  - block level
  - grid level

  我们可以决定某个算法的并行层次。例如对于sparse conv3d的backprop filter来说，是每个warp处理一个点的所有input channel，还是每个thread处理一个单独的input channel，等等。同一个点的所有input channel它们会有相似的特征，从而使控制流的执行完全一致（例如分支预测结果总是一致，循环迭代次数一致等），因此当channel数量是32的倍数时用一个warp来执行效率会很高。但是当channel数量不是32的倍数时就会浪费掉一些线程的计算资源。这是一个trade off。另外，每个层次上都有各自的同步机制，层次越低同步开销越小
- 在7.0 compute cabability上一个时钟周期可以执行：
  - 32次32位整数加法或乘法，？？或
  - ？次半精度（16位）浮点数乘法，或
  - ？次单精度（32位）浮点数乘法，或
  - ？次双精度（64位）浮点数乘法

  等等。具体参考？？
- 整数除法的cost大约是加法或乘法的20倍，因为会被分解成约20条指令
- 实验表明某些情况下if分支和除法的效率一样差
- global memory的一次读取（非cache）耗费400到600个时钟周期？？？
- 浮点数加法不满足结合律！
- 使用ldg()读取readonly多次的数据！
