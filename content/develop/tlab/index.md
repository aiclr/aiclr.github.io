+++
title = 'TLAB'
date = '2024-07-05'
author = 'aiclr'
categories = ['develop']
tags = ['jvm','java','tlab']
Summary='Thread Local Allocation Buffer'
+++

`Thread Local Allocation Buffer`。`OpenJDK`衍生出来的`JVM`都提供了`TLAB`的设计。\
从**内存模型**而不是`GC`角度，对`Eden`区域继续进行划分，`JVM`为每个线程分配了一个**私有缓存区域**，它包含在`Eden`空间内。\
**多线程**同时分配内存时，使用`TLAB`可以避免一系列的非线程安全问题，同时还能够**提升内存分配吞吐量**，因此我们可以将这种内存分配方式称为**快速分配策略**。\
不是所有的对象实例都能够在`TLAB`中成功分配内存，但`JVM`确实将`TLAB`作为内存分配的首选。\
在程序中，可以通过选项 `-XX:+UseTLAB` 设置开启 `TLAB` 空间。**默认开启**。\
默认情况下，`TLAB`空间的内存非常小仅占整个`Eden`区的`1%`，`-XX:TLABWasteTargetPercent`设置`TLAB`空间所占用`Eden`空间的百分比大小。\
一旦对象在`TLAB`空间分配**内存失败**时，`JVM`会尝试通过**加锁机制**确保数据操作的**原子性**，从而直接在`Eden`空间中分配内存。

为什么有 `TLAB`
- `Heap Area`是线程共享区域，任何线程都可以访问到堆区中的共享数据
- 由于对象实例的创建在`JVM`中非常频繁，因此在**并发环境**下从`Heap Area`中划分内存空间是**线程不安全**的
- 为避免多个线程操作同一地址，需要使用加锁等机制，会影响内存分配速度

## 指针碰撞

todo