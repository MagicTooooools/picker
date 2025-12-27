---
title: Actor 模型介绍
url: https://www.ttalk.im/articles/RgeKNT
source: The Talk
date: 2025-12-26
fetch_date: 2025-12-27T03:24:48.330938
---

# Actor 模型介绍

[The Talk](/)

![Actor 模型介绍](/files/snDrTH/823600278b8d4d71)

# Actor 模型介绍

[![david](/files/snDrTH/5a4d41de51604174)david](/u/snDrTH)●

2025年12月26日

●

25 次阅读

[Erlang](/u/snDrTH?tag=ZqMVU0)[杂谈](/u/snDrTH?tag=Ejh8xT)

Actor模型并发编程详解：深入理解Carl Hewitt提出的Actor模型原理，对比Erlang与Akka两种实现的调度机制、GC策略及IO操作差异

## Actor模型

### Actor模型定义

Actor模型可以说是并发编程中非常常见的一种模型，该模型是Carl Hewitt在1973年提出的。 Actor模型是一种程序上的抽象概念，被视为并发运算的基本单元：当一个Actor收到一条消 息，它可以作出决策，建立更多的Actor，传递消息以及决定如何回复接收到消息。每个 Actor都拥有自己的私有状态，一个Actor想要改变另一个Actor的状态，只能通过发送消息 的方式来完成。

### Actor模型如何工作

1. Actor是独立的，每个Actor只管理自己的内部数据，对外暴露一个通信用的邮箱
2. Actor都是通过向另一Actor暴露的邮箱发送消息来进行通信
3. Actor之间的通讯行为是异步的

### Actor模型中的Actor可以做什么

1. 接收消息并进行相应处理
2. 对接收到的消息进行回复
3. 更新自己的私有状态
4. 创建新的Actor
5. 发送消息给另一个Actor

## Actor实现

### Erlang的Actor实现

1. Erlang的Actor模型是基于Erlang进程实现的
2. Erlang的Actor在死亡后会立刻进行垃圾处理
3. Erlang的Actor直被Erts内部的POSIX线程调度

### Akka的Actor实现

1. Akka的Actor模型以类为基础，通过Java的类库来实现
2. Akka的Actor在死亡后，需要等待JVM进行GC时才进行垃圾处理
3. Akka的Actor是通过Java的线程池调度

### 两种实现的差异

#### Actor调度

Erlang的调度方式是抢占式公平调度（Erts强行切换），Akka的调度是协作式调度（完全依 赖Actor主动放弃调度器）。Erlang的Actor调度会受到CPU数量和Actor数量影响，具体例子 可以看下面：

* 我们的CPU为2Core时，process数为200，每个process平均获得CPU的能力 1/200 ＊ 2 ＝ 1% 。
* 我们的CPU为4Core时，process数为200，每个process平均获得CPU的能力 1/200 ＊ 4 ＝ 2% 。
* 我们的CPU为8Core时，process数为1000，每个process平均获得CPU的能力 1/1000 \* 8 Core = 0.8%。

也就是说，Erlang在进程数不变的时候，增加CPU会增加每个Erlang进程的执行时，而Akka 的调度是协作式的调度，这就代表着我们无法得到上面例子中的算式，当一个调度器上的某 个Actor在做一个非常长时间的计算，完全由可能让调度器上的其它Actor不能按时调度。

#### IO操作

Erlang的Actor在执行IO的时候会进入等待状态，放弃调度线程，Akka在不使用封装后的IO操作时会一直占用调度线程，使用封装的IO操作时才会放弃调度线程。 Erlang因为是Erts提 供的IO操作，相对会比较统一，但是如果使用自己编写的Nif或Driver就需要注意是否存在 同步的IO操作，因为这种原生的IO操作会让Actor一直占用调度线程。 因此在Akka的Actor 中，尽量不要使用Java提供的同步IO操作，而应该使用Akka提供的异步IO操作。

### 关于博客

探索分享未知的知识，记录快乐的每一天

© 2025 The Talk. All rights reserved.