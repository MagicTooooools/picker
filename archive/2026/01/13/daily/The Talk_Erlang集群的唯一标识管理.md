---
title: Erlang集群的唯一标识管理
url: http://www.ttalk.im/articles/HbVetj
source: The Talk
date: 2026-01-13
fetch_date: 2026-01-14T03:39:23.646265
---

# Erlang集群的唯一标识管理

[The Talk](/)

[登录](/admin/login)

![Erlang集群的唯一标识管理](/files/snDrTH/f_ms/thumb-small)

目录

* [Erlang中的global和local名字](#erlang中的global和local名字 "Erlang中的global和local名字")
* [Erlang进程的名字](#erlang进程的名字 "Erlang进程的名字")
* [gen\_server是如何创建进程](#gen_server是如何创建进程 "gen_server是如何创建进程")
* [local和global的区别](#local和global的区别 "local和global的区别")
* [Global模块分析](#global模块分析 "Global模块分析")
* [global模块功能](#global模块功能 "global模块功能")
* [global模块启动](#global模块启动 "global模块启动")
* [global名字注册](#global名字注册 "global名字注册")

# Erlang集群的唯一标识管理

[![david](/files/snDrTH/5a4d41de51604174/thumb-avatar)
david](/u/snDrTH)
●

2026年1月13日

●

96 次阅读

[Erlang](/u/snDrTH?tag=ZqMVU0)

Erlang是一个非常强大的分布式平台，其中一大特点就是可以为集群中任意一个进程添加全 集群唯一标识。本文通过分析Erlang/OTP的代码，来介绍Erlang是如何完成全集群唯一名标 管理。也就是常说的进程名字管理。

## Erlang中的global和local名字

在开发Erlang/OTP程序的时候，看到最多的就是gen\_server，在调用 gen\_server:start\_link是，经常会看到{global,?MODULE}或{local,?MODULE}。那么这之间 有什么差异呢？

### Erlang进程的名字

Erlang在创建的进程的时候，给予Erlang进程一个PID作为进程的标识。那么经常使用的命 名进程是怎么来的呢？是调用`erlang:register`这个函数将原子和PID进行关联，从而产生了 命名的Erlang进程。而`erlang:register`函数接收的第一个参数可以看到是一个原子，而不 是一个元组。那么gen\_server为什么会使用一个元组呢？

### gen\_server是如何创建进程

先看下gen\_server:start\_link的代码

```
start_link(Name, Mod, Args, Options) ->
    gen:start(?MODULE, link, Name, Mod, Args, Options).
```

从这里看到gen\_server是调用gen模块进行进程创建的，那么gen模块又是如何创建进程的：

```
-spec start(module(), linkage(), emgr_name(), module(), term(), options()) ->
    start_ret().

start(GenMod, LinkP, Name, Mod, Args, Options) ->
    case where(Name) of
    undefined ->
        do_spawn(GenMod, LinkP, Name, Mod, Args, Options);
    Pid ->
        {error, {already_started, Pid}}
    end.

-spec start(module(), linkage(), module(), term(), options()) -> start_ret().

start(GenMod, LinkP, Mod, Args, Options) ->
    do_spawn(GenMod, LinkP, Mod, Args, Options).

%%-----------------------------------------------------------------
%% Spawn the process (and link) maybe at another node.
%% If spawn without link, set parent to ourselves 'self'!!!
%%-----------------------------------------------------------------
do_spawn(GenMod, link, Mod, Args, Options) ->
    Time = timeout(Options),
    proc_lib:start_link(?MODULE, init_it,
            [GenMod, self(), self(), Mod, Args, Options],
            Time,
            spawn_opts(Options));
do_spawn(GenMod, _, Mod, Args, Options) ->
    Time = timeout(Options),
    proc_lib:start(?MODULE, init_it,
           [GenMod, self(), self, Mod, Args, Options],
           Time,
           spawn_opts(Options)).

do_spawn(GenMod, link, Name, Mod, Args, Options) ->
    Time = timeout(Options),
    proc_lib:start_link(?MODULE, init_it,
            [GenMod, self(), self(), Name, Mod, Args, Options],
            Time,
            spawn_opts(Options));
do_spawn(GenMod, _, Name, Mod, Args, Options) ->
    Time = timeout(Options),
    proc_lib:start(?MODULE, init_it,
           [GenMod, self(), self, Name, Mod, Args, Options],
           Time,
           spawn_opts(Options)).
```

可以清楚的看到，使用的proc\_lib，而proc\_lib是对`erlang:spawn_link`进行封装，以确保 初始化函数能正确运行，那么注册名字的秘密就在`gen:init_it`中。在`gen:init_it`中可以看 到一个内部函数name\_register。

```
name_register({local, Name} = LN) ->
    try register(Name, self()) of
    true -> true
    catch
    error:_ ->
        {false, where(LN)}
    end;
name_register({global, Name} = GN) ->
    case global:register_name(Name, self()) of
    yes -> true;
    no -> {false, where(GN)}
    end;
name_register({via, Module, Name} = GN) ->
    case Module:register_name(Name, self()) of
    yes ->
        true;
    no ->
        {false, where(GN)}
    end.
```

此时此刻，可以看到global和local的明显差异。

### local和global的区别

从上面的代码和对Erlang虚拟机的跟踪可以知道，`erlang:register`管理的名字和进程PID关 联表只是调用者本地的Erlang虚拟机内的，不是整个集群中的。而`global:register_name`是 通过global模块对集群中所有Erlang虚拟机进行操作。从这可以看出，Erlang语言本身并没 有所谓本地名字或集群名字的概念，而这个概念是OTP当中的（但是Erlang有本地节点进程 和远程节点进程的概念）。

## Global模块分析

### global模块功能

1. 管理全局名字
2. 管理全局锁
3. 维护Erlang集群的互联互通

### global模块启动

该模块是在Erlang节点启动的时候自动被启动的，并且会组册一个名为global\_name\_server 的进程。并且需要注意的是global模块本身就是一个gen\_server，不过为了避免死循环， global模块使用gen\_server注册的是本地名字。在global进程创建成功后，建立了大量的 ets表，其中global\_names表，global\_pid\_names表就是用来管理全局命名的。

### global名字注册

注册名字的时候，就是让所有节点执行{register,Name,Pid,Method}。可以看下面这段代码：

```
register_name(Name, Pid) when is_pid(Pid) ->
    register_name(Name, Pid, fun random_exit_name/3).

register_name(Name, Pid, Method0) when is_pid(Pid) ->
    Method = allow_tuple_fun(Method0),
    Fun = fun(Nodes) ->
        case (where(Name) =:= undefined) andalso check_dupname(Name, Pid) of
            true ->
                gen_server:multi_call(Nodes,
                                      global_name_server,
                                      {register, Name, Pid, Method}),
                yes;
            _ ->
                no
        end
    end,
    ?trace({register_name, self(), Name, Pid, Method}),
    gen_server:call(global_name_server, {registrar, Fun}, infinity).
```

当gobal进程收到了{register,Name,Pid,Method}消息后，会向在global进程建立时建立的 另一个无名进程发送消息{trans\_all\_known, Fun, From}，这个无名进程的代码如下：

```
loop_the_registrar() ->
    receive
        {trans_all_known, Fun, From} ->
            ?trace({loop_the_registrar, self(), Fun, From}),
            gen_server:reply(From, trans_all_known(Fun));
    Other ->
            unexpected_message(Other, register)
    end,
    loop_the_registrar().

unexpected_message({'EXIT', _Pid, _Reason}, _What) ->
    %% global_name_server died
    ok;
unexpected_message(Message, What) ->
    error_logger:warning_msg("The global_name_server ~w process "
                             "received an unexpected message:\n~p\n",
                             [What, Message]).
```

这个进程会使用trans\_all\_known来执行传入的函数，trans\_all\_known函数代码如下：

```
trans_all_known(Fun) ->
    Id = {?GLOBAL_RID, self()},
    Nodes = set_lock_known(Id, 0),
    try
%当锁住了所有的节点，才执行相关的操作
%全局的大锁呀，用多了性能还是比较差的
        Fun(Nodes)
    after
        delete_global_lock(Id, Nodes)
    end.

set_lock_known(Id, Times) ->
    Known = get_known(),
    Nodes = [node() | Known],
%Boss是List中最后的那个元素
    Boss = the_boss(Nodes),
    %% Use the  same convention (a boss) as lock_nodes_safely. Optimization.
%先锁定住Boss
    case set_lock_on_nodes(Id, [Boss]) of
        true ->
%接这锁住剩下的节点
            case lock_on_known_nodes(Id, Known, Nodes) of
                true ->
                    Nodes;
                false ->
                    del_lock(Id, [Boss]),
                    random_sleep(Times),
                    set_lock_known(Id, Times+1)
            end;
        false ->
            random_sleep(Times),
            set_lock_known(Id, Times+1)
    end.

lock_on_known_nodes(Id, Known, Nodes) ->
    case set_lock_on_nodes(Id, Nodes) of
        true ->
            (get_known() -- Known) =:= [];
        false ->
            false
    end.

set_lock_on_nodes(_Id, []) ->
    true;
set_lock_on_nodes(Id, Nodes) ->
    case local_lock_check(Id, Nodes) of
        true ->
            Msg = {set_lock, Id},
            {Replies, _} =
                gen_server:multi_call(Nodes, global_name_server, Msg),
            ?trace({set_lock,{me,self()},Id,{nodes,Nodes},{replies,Replies}}),
            check_replies(Replies, Id, Replies);
        false=Reply ->
            Reply
    end.
```

可以看出执行流程是这样的，先锁住集群中排序最大的那个节点，如上锁成功，则让所有的 其余节点跟着上锁，如果上锁失败，则随机睡眠一段时间再接着尝试。如果当所有节点上都 拿到锁，就执行名字注册，并且执行注册后。由于使用try after语句进行包裹，在执行最 后一定会释放锁。

为什么这样上锁 首先全局的锁（GLOBAL\_RID）是所有节点共享的，如果从随机的一个节点开始上锁，很容易 出现同时好几个节点都在上锁而发生锁冲突，那么大家就约定先上锁某一个节点，这样能快 速的发现锁的冲突。 其次，因为要在没给节点上的ets表中添加一个记录，如果不能在所有 参与节点上添加记录，会出现数据不一致的问题。 最后，不能只锁定一个约定的节点，考 虑到不稳定性，当节点出现异常无法连通的时候，那么这个锁的机制就无效了。

### 目录

* [Erlang中的global和local名字](#erlang中的global和local名字 "Erlang中的global和local名字")
* [Erlang进程的名字](#erlang进程的名字 "Erlang进程的名字")
* [gen\_server是如何创建进程](#gen_server是如何创建进程 "gen_server是如何创建进程")
* [local和global的区别](#local和global的区别 "local和global的区别")
* [Global模块分析](#global模块分析 "Global模块分析")
* [global模块功能](#global模块功能 "global模块功能")
* [global模块启动](#global模块启动 "global模块启动")
* [global名字注册](#global名字注册 "global名字注册")

### 关于博客

探索分享未知的知识，记录快乐...