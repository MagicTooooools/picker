---
title: Erlang 是如何释放NIF的资源
url: https://www.ttalk.im/articles/iNDHf9
source: The Talk
date: 2025-12-29
fetch_date: 2025-12-30T03:32:20.246296
---

# Erlang 是如何释放NIF的资源

[The Talk](/)

# Erlang 是如何释放NIF的资源

[![david](/files/snDrTH/5a4d41de51604174/thumb-avatar)david](/u/snDrTH)●

2025年12月29日

●

39 次阅读

[Erlang](/u/snDrTH?tag=ZqMVU0)

深入解析 Erlang NIF 资源释放机制。通过 Lua 插件开发实践，详解如何使用 enif\_alloc\_resource 分配资源、enif\_make\_resource 与 enif\_release\_resource 实现引用计数管理，确保进程崩溃时自动释放 NIF 绑定的 C 资源。对比 Port 方案，NIF 是 Erlang 调用 C 代码最高效的方式，本文帮助开发者掌握其内存安全的核心原理。

## 什么是NIF

NIF是Native Implemented Function的缩写，从词面意思就可以看出，NIF是Erlang使用平 台相关语言（一般是C语言）在Erts层面实现操作系统或外部库沟通的方案。

## NIF和Ports的异同

### 相同点

NIF和Ports 都是为Erts可以同操作系统和外部库进行通讯，给使用者在不改变Erts代码的 前提下扩展Erts的功能。

### 不同点

Ports可以被分为普通Port和Port Driver。 普通Port是连接外部程序进程和Erts的桥梁，外部进程通过标准输入输出与Erts虚拟机交互， 并运行于独立的地址空间。它有以下特点：

1. 从操作系统的角度看，外部程序和Erlang虚拟机都是独立运行的进程，Erts通过外部程 序的标准输入输出与外部程序教务， 因此外部程序的崩溃不会影响到Erts本身的正常运 行。
2. 当外部程序崩溃了，Erlang虚拟机可以检测到，可以选择重启等对应策略。由于两者在 不同的地址空间，通过标准IO交互
3. 每个Port都有一个owner进程，通常为创建Port的进程，当owner进程终止时，Port也将 被自动关闭。

可以看出普通Port的优势在于隔离性和安全性，因为外部程序的任何异常都不会导致Erts崩溃，并且Erlang层通过receive来实现同步调用等待外部程序响应时，是不会影响Erts的调度器调度Erlang进程。 读者也可以看出Port的缺点，就是效率低，因为是操作系统中两个程序通过标准输入输出传 递字节流数据，所以需要额外的序列化和反序列化。 同时外部程序是Erts通过系统fork调 用或者exec调用创建出来的，因此创建Port的资源代价是很大的。

Port Driver 从Erlang的角度来看，端口驱动和普通Port所体现的行为模式一样，收发消息， 注册名字，并且使用和普通Port相同的语义，因此Port Driver也具有一个Owner进程。 但 是端口驱动本身是作为一个链接库运行于Erts中的，也就是和Erts共享一个操作系统进程， 因此Port Driver的性能比普通的Port提高了不少。 但是缺点也非常明显，链入的动态链接 库本身可能出现内存泄露或异常，将影响Erts正常运行甚至导致虚拟机崩溃。

NIF是Erlang调用C代码最简单高效的方案，对Erlang层来说，调用NIF就像调用普通函数一 样，只不过这个函数是由C实现的。 NIF是同步语义的，运行于调度线程中，无需上下文切 换，因此效率很高。但是对于执行时间长的NIF，在NIF返回之前，调度线程不能做别的事情， 影响了虚拟机的公平调度，甚至会影响调度线程之间的协作。

从Erlnag层面上Port Driver和NIF非常相似，但是NIF并没有所谓的Owner进程，因为它只是 一组函数。 Port Driver是可以监控Owner进程的存活状态，当Owner进程终止时，Port Driver也会终止，并释放所拥有的资源。 那么就引出本文题目的问题了。

## 为什么会产生NIF资源

从前面的描述，读者已经可以清楚的看到NIF的优势，NIF更加体现函数化编程的思想，NIF 自身并不和任何Erlang进程绑定，是完全可以重入的，可以被任何一个Erlang进程在任何时 间调用，并且在Erts的调度线程上执行。 但是很多时候，NIF仍需要产生上下文供函数使用， 这种上下文就是所说的资源，而这种资源很可能是需要在调用者退出时进行释放的。 举例 子来说，在开发[AiLua](/s/HbVetj)的时候，其中NIF需要将Lua虚拟机作为资源和调用者的Erlang进程绑 定。当Erlang进程不管是正常退出还是异常退出，都需要释放这个资源，从而防止内存泄漏。

## 解决方法

这个时候，我想起了一个伟大的工程eleveldb。leveldb也是需要打开句柄，并且相应的进 程也使用这个句柄。所以就去翻看了eleveldb的代码。发现几个非常重要的函数

```
ErlNifResourceType *enif_open_resource_type(ErlNifEnv* env, const char* module_str, const char* name,
 ErlNifResourceDtor* dtor, ErlNifResourceFlags flags, ErlNifResourceFlags* tried);

void *enif_alloc_resource(ErlNifResourceType* type, unsigned size);

ERL_NIF_TERM enif_make_resource(ErlNifEnv* env, void* obj);

void enif_release_resource(void* obj);
```

这几个函数是怎么解决资源释放问题的呢？

我们可以看到eleveldb在on\_load的时候，使用enif\_open\_resource\_type创建了一个资源类 型。之后在每次请求打开DB的时候，通过i下面方式进行NIF资源和Erlang进程绑定

1. 用enif\_alloc\_resource分配出一个资源.
2. 用enif\_make\_resource和enif\_release\_resource将资源的控制权交给了调用的进程

## 实现方式

Erts对NIF资源的管理，使用了最简单的资源计数的方式来进行管理。将一个NIF的资源和 Erlang进程绑定需要以下几个步骤

1. 使用enif\_alloc\_resource在分配资源的时候，分配的资源引用计数为1，并且该资源不 属于任何一个Erlang的进程。
2. 使用enif\_make\_resource的时候，相当于在调用者的堆栈上分配了一个指向资源的指针， 并将资源的引用计数加1。
3. 使用enif\_release\_resource将资源的引用计数减少1，这样就相当将资源的控制权交给 了调用的Erlang进程。

那么资源是怎么安全释放的呢？

这个时候我们可以看到资源的引用计数为1，并且这个引用者是Erlang进程堆。我们都知道 Erlang进程崩溃后，Erts会清理Erlang进程堆和栈，释放资源。 那么当Erlang进程崩溃后， 就会调用enif\_open\_resource\_type被调用的时候所传入的析构函数，而这个析构函数的参 数就是前面所分配的资源。

### 关于博客

探索分享未知的知识，记录快乐的每一天

© 2025 The Talk. All rights reserved.