---
title: Erlang 的Port Drivers
url: https://www.ttalk.im/articles/ZqMVU0
source: The Talk
date: 2025-12-26
fetch_date: 2025-12-27T03:24:48.658714
---

# Erlang 的Port Drivers

[The Talk](/)

![Erlang 的Port Drivers](/files/snDrTH/4b0a44f00c4f49bc)

# Erlang 的Port Drivers

[![david](/files/snDrTH/5a4d41de51604174)david](/u/snDrTH)●

2025年12月26日

●

20 次阅读

[Erlang](/u/snDrTH?tag=ZqMVU0)

详解 Erlang Port 与 Port Drivers 的区别：Port 通过独立进程与外部通信，Port Drivers 将 C/C++ 动态库加载到虚拟机内，性能更高但风险更大。深入剖析异步事件驱动调度机制与软实时设计原理。

## 什么是Port

Port可以说是Erlang提供的一种和Erts虚拟机以外世界通信的最基本的方式。Ports为 Erlang提供了双向字节流，让Erlang可以非常好的和外部程序进行通信。默认情况下，外部 程序会在一个全新的OS进程中运行，Erlang通过外部程序的标准输入（文件句柄0）和标准 输出（文件句柄1）进行通讯。创建Port的Erlang进程一般我们将它成为，Port的所有者， 所有和Port的通信都会通过Port所有者进程进行，当所有者进程停止运行了，外部程序也应该退出。

## 什么是Port Drivers

当然我们也可以用C或者C++编写一个动态链接库文件（.so或者.dll）让Erlang动态的加载 到虚拟机内，这个动态链接库不会创建新的OS进程，而是直接使用Erlang的进程。但是， Erlang依然使用和Ports，我们将这种内联的Port称为Port Drivers。

## Port和Port Drivers的差异

创建方式的不同：Port是通过`erlang:open_port`直接来创建的，而Port Drivers是先要通过 `erl_ddll:load_driver`加载到虚拟机内后再通过`erlang:open_port`完成创建的。

运行方式的不同：Port是运行在Erlang虚拟机外的OS进程和Erlang虚拟机不共享进程，不会 引起Erlang虚拟机内存泄露和崩溃，而Port Drivers和Erlang的虚拟机共享进程，如果处理 不当会引起Erlang虚拟机的崩溃和内存泄露。

性能的差异：Port在创建的时候，beam.smp会使用vfork复制整个进程，这个会导致整个 beam.smp进程阻塞，而Port Drivers只是创建一堆数据，所以性能不用说。

## Port Drivers是如何调度的

在`erl_port_task.c`中我们可以找到`erts_port_task_schedule`函数，正式这个函数将Port Driver调度到Erlang虚拟机上的scheduler上的。`erts_port_task_schedule`函数会在 `erl_check_io`的时候被调用。从这些代码中我们可以观察到：

1. Port Drivers并不会一直放在ErtsPortTaskSched当中。
2. Erlang的Port Drivers只有在Erlang进程通过`erlang:control`和`erlang:command`函数发 送命令时，会将Port放入RunQueue。
3. Erlang的Port Drivers向Erlang虚拟机注册IO任务，erts会在`erl_check_io`放到 RunQueue中。

## Port Drivers为什么这么实现和调度

1. Erlang的虚拟机的调度器是一个软实时的调度器，它在调度Erlang进程的时候会为 Erlang进程分配固定的reduction。Erlang虚拟机规定了每个Erlang的操作的reduction 的数量，当Erlang进程的reduction减少到位0的时候，将进行Erlang进程切换。
2. Erlang的虚拟机要保证调度器是无阻塞的，才能达到软实时调度。通过reduction机制， 可以保证不执行IO操作的Erlang进程达到无阻塞。为了让IO操作不阻塞调度器，那么就 必须让IO操作变成一种异步任务。
3. Port Drivers很多时候，都是为了完成外部通信操作或者IO操作。因此Erlang将所有的 IO操作都和事件驱动进行关联，当不能直接向事件驱动器的注册的IO操作则通过异步线程模拟成IO事件。这样就可以将IO操作变成IO任务，这样就如同无阻塞的操作一样。
4. Port Drivers可以说是Erlang虚拟机对IO操作的高级抽象，这样就将复杂的外部世界和 非IO操作尽最大可能的隔离开了。不但可大大减少代码量，同时也提高了平台的兼容性。 最终Erlang的虚拟机内，将所有的IO操作和计算抽象成了执行队列上的一个又一个任务， 方便运行在多核心上的调度器进行调度和任务密取，提高并发。

### 关于博客

探索分享未知的知识，记录快乐的每一天

© 2025 The Talk. All rights reserved.