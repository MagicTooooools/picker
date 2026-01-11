---
title: LangGraph 是如何让LLM产生确定性输出的？
url: https://www.luozhiyun.com/archives/896
source: luozhiyun`s Blog
date: 2026-01-10
fetch_date: 2026-01-11T03:45:03.108736
---

# LangGraph 是如何让LLM产生确定性输出的？

Search for:

Search

[Skip to content](#content)

[luozhiyun`s Blog](https://www.luozhiyun.com/)

我的技术分享

Menu

* [首页](http://www.luozhiyun.com/)
* [Go系列](https://www.luozhiyun.com/archives/category/%E5%90%8E%E7%AB%AF/go)
* [kubernetes源码系列](https://www.luozhiyun.com/archives/tag/%E6%B7%B1%E5%85%A5k8s)
* [关于我](https://www.luozhiyun.com/%E5%85%B3%E4%BA%8E)
* [RSS订阅](https://www.luozhiyun.com/feed)
* [Github](https://github.com/luozhiyun993)

# LangGraph 是如何让LLM产生确定性输出的？

Posted on 2026年1月10日 by [luozhiyun](https://www.luozhiyun.com/archives/author/luoluo1993)

像经常用 LLM 的同学都知道现在最头疼的问题就是幻觉问题，在金融或精密计算领域，不确定性意味着风险。 如果 Agent 负责分析 NVDA 或 TSLA 的财报，开发者希望它在处理相同数据时，逻辑推导链条是严密的，而不是在不同时间给出自相矛盾的结论。或是需要 LLM 输出 JSON 来触发一个 API，我们不会希望 LLM 在 JSON 里多加了一个逗号或改变了字段名。

最后我还尝试用 LangGraph 的理念自己写了一个 **[smallest-LangGraph](https://github.com/luozhiyun993/smallest-LangGraph)**，

## LangGraph 可以做什么？

传统的 LangChain 核心逻辑是 **DAG（有向无环图）**。我们可以轻松定义 `A -> B -> C` 的步骤，但如果你想让 AI 在 `B` 步骤发现结果不满意，自动**跳回** `A` 重新执行，LangChain 的普通 Chain 很难优雅地实现。并且在复杂的长对话或多步骤任务中，维护一个全局的、可持久化的“状态快照”非常困难。

所以为了解决这些问题，LangGraph 就诞生了。LangGraph 的主要有这些核心优势：

1. 支持“循环（Cycles）”与“迭代”

   **思考** -> 2. **行动** -> 3. **观察结果** -> 4. **如果不满意，回到第1步**。 LangGraph 允许你定义这种闭环逻辑，这在长任务、自我修正代码、多轮调研场景下是刚需。
2. 状态管理

   LangGraph 引入了 `State` 的概念，所有节点共享同一个 `TypedDict`，你可以精确定义哪些数据是追加的（`operator.add`），哪些是覆盖的。并且它可以自动保存每一步的状态。即使程序崩溃或需要人工审核，你也可以从特定的“存档点”恢复，而不需要从头运行。
3. 人机协作

   LangGraph 允许你将流程设计为“在某处强制停下”，等待人类信号后再继续。这在 LangChain 的线性模型中极难实现，但在 LangGraph 的状态机模型中只是一个节点属性。
4. 高度可控

   “如果工具返回报错，必须走 A 路径。” 这种**确定性**对于生产环境的后端服务至关重要。不能让模型乱输出，在生产环境上严格把控输出结果是很重要的。

## LangGraph 结构

由于 LangGraph 的核心思想是将 Agent 的工作流建模为一张**有向图（Directed Graph）**。所以 LangGraph 有如下几个结构组成

* 全局状态（State）

  这个状态通常被定义为一个 Python 的 `TypedDict`，它可以包含任何你需要追踪的信息，如对话历史、中间结果、迭代次数等，所有的节点都能读取和更新这个中心状态。
* 节点（Nodes）

  每个节点都是一个接收当前状态作为输入、并返回一个更新后的状态作为输出的 Python 函数。
* 边（Edges）

  边负责连接节点，定义工作流的方向。最简单的边是常规边，它指定了一个节点的输出总是流向另一个固定的节点。而 LangGraph 最强大的功能在于**条件边（Conditional Edges）**。它通过一个函数来判断当前的状态，然后动态地决定下一步应该跳转到哪个节点。

基于上面的概念，我们来做一个例子，假设我们要开发一个 Agent：它先翻译一段话，然后自己检查是否有语法错误，如果有，就打回重新翻译；如果没有，就结束。

首先，我们先定义状态 (State)：

```
from typing import TypedDict, List

class AgentState(TypedDict):
    # 原始文本
    input_text: str
    # 翻译后的文本
    translated_text: str
    # 反思反馈
    feedback: str
    # 循环次数（防止死循环）
    iterations: int
```

定义节点逻辑 (Nodes)：

```
def translator_node(state: AgentState):
    print("--- 正在翻译 ---")
    # 这里通常会调用 LLM
    new_text = f"Translated: {state['input_text']}"
    return {"translated_text": new_text, "iterations": state.get("iterations", 0) + 1}

def critic_node(state: AgentState):
    print("--- 正在自检 ---")
    # 模拟检查逻辑，如果包含 'bad' 字符就认为不合格
    if "bad" in state['translated_text']:
        return {"feedback": "发现不当词汇，请重试"}
    return {"feedback": "OK"}
```

定义路由逻辑 (Conditional Edges)：

```
def should_continue(state: AgentState):
    if state["feedback"] == "OK" or state["iterations"] > 3:
        return "end"
    else:
        return "rephrase"
```

构建图 (Graph Construction)：

```
from langgraph.graph import StateGraph, END

# 1. 初始化图
workflow = StateGraph(AgentState)

# 2. 添加节点
workflow.add_node("translator", translator_node)
workflow.add_node("critic", critic_node)

# 3. 设置入口点
workflow.set_entry_point("translator")

# 4. 连接节点
workflow.add_edge("translator", "critic")

# 5. 添加条件边 (根据 critic 的反馈决定去向)
workflow.add_conditional_edges(
    "critic",
    should_continue,
    {
        "rephrase": "translator", # 如果不 OK，回到翻译节点
        "end": END                # 如果 OK，结束
    }
)

# 6. 编译成可执行应用
app = workflow.compile()
```

通过上面这种编排方式，可以让 LLM 概率性输出产生确定性的输出，通过各种限制节点，很好的控制了 LLM 的访问的节点。

下面我给出完整的例子，大家可以用这个例子去尝试一下：

```
from typing import TypedDict, List
from langchain_openai import ChatOpenAI
from langgraph.graph import StateGraph, END
from langchain_core.messages import SystemMessage, HumanMessage

llm = ChatOpenAI(
    temperature=0.6,
    model="glm-4.6V",
    openai_api_key="",
    openai_api_base="https://open.bigmodel.cn/api/paas/v4/"
)

class AgentState(TypedDict):
    # 原始文本
    input_text: str
    # 翻译后的文本
    translated_text: str
    # 反思反馈
    feedback: str
    # 循环次数（防止死循环）
    iterations: int

def translator_node(state: AgentState):
    """翻译节点：负责将中文翻译成英文"""
    print(f"\n--- [节点：翻译器] 第 {state.get('iterations', 0) + 1} 次尝试 ---")

    iters = state.get("iterations", 0)
    feedback = state.get("feedback", "无")

    # 构建提示词：如果是重试，带上反馈建议
    system_prompt = "你是一个专业的翻译官。请将用户的中文翻译成地道、优雅的英文。"
    if iters > 0:
        system_prompt += f" 注意：这是第二次尝试，请参考之前的反馈进行改进：{feedback}"

    response = llm.invoke([
        SystemMessage(content=system_prompt),
        HumanMessage(content=state["input_text"])
    ])

    return {
        "translated_text": response.content,
        "iterations": iters + 1
    }

def critic_node(state: AgentState):
    """评审节点：检查翻译质量"""
    print("--- [节点：评审员] 正在检查翻译质量... ---")

    system_prompt = (
        "你是一个严苛的英文编辑。请评价以下翻译是否准确、地道。"
        "如果翻译得很好，请只回复关键词：【PASS】。"
        "如果翻译有改进空间，请直接指出问题并给出改进建议。"
    )

    user_content = f"原文：{state['input_text']}\n译文：{state['translated_text']}"

    response = llm.invoke([
        SystemMessage(content=system_prompt),
        HumanMessage(content=user_content)
    ])

    return {"feedback": response.content}

# 4. 定义路由逻辑
def should_continue(state: AgentState):
    """判断是继续修改还是直接结束"""
    if "【PASS】" in state["feedback"] or state["iterations"] >= 3:
        if state["iterations"] >= 3:
            print("!!! 达到最大尝试次数，停止优化。")
        return "end"
    else:
        print(f">>> 反馈建议：{state['feedback']}")
        return "rephrase"

# 1. 初始化图
workflow = StateGraph(AgentState)

# 2. 添加节点
workflow.add_node("translator", translator_node)
workflow.add_node("critic", critic_node)

# 3. 设置入口点
workflow.set_entry_point("translator")

# 4. 连接节点
workflow.add_edge("translator", "critic")

# 5. 添加条件边 (根据 critic 的反馈决定去向)
workflow.add_conditional_edges(
    "critic",
    should_continue,
    {
        "rephrase": "translator", # 如果不 OK，回到翻译节点
        "end": END                # 如果 OK，结束
    }
)

# 6. 编译成可执行应用
app = workflow.compile()

# 7. 运行时交互
if __name__ == "__main__":
    print("=== LangGraph 智能翻译 Agent (输入 'exit' 退出) ===")
    while True:
        user_input = input("\n请输入想要翻译的中文内容: ")
        if user_input.lower() == 'exit':
            break

        # 初始状态
        initial_state = {
            "input_text": user_input,
            "iterations": 0
        }

        # 运行图并获取最终状态
        final_state = app.invoke(initial_state)

        print("\n" + "=" * 30)
        print(f"最终翻译结果：\n{final_state['translated_text']}")
        print("=" * 30)
```

## LangGraph 是如何管理状态的？

### State Reducer 自动合并 state

Reducer 在 LangGraph 中就是一种更新状态的处理逻辑，如果没有指定默认行为是 **用新值覆盖旧值**。想要指定 Reducer 只需要通过 typing.Annotated 字段绑定一个 Reducer 函数即可。

比如使用 operator.add 定义这是一个“追加型”字段：

```
from typing import Annotated, TypedDict
from langgraph.graph import StateGraph, END
import operator

# 定义状态结构 (类似 Go 的 Struct)
class AgentState(TypedDict):
    # 使用 Annotated 和 operator.add 定义这是一个“追加型”字段
    # 每次节点返回消息，都会 append 到这个列表，而不是覆盖它
    messages: Annotated[list[str], operator.add]

    # 普通字段，默认行为是 Overwrite (覆盖)
    # 适合存储状态机当前的步骤或分析结论
    current_status: str

    # 计数器，也可以使用 operator.add 实现增量累加
    retry_count: Annotated[int, operator.add]
```

### Checkpointer + Thread 持久化状态

在 LangGraph 中，**Checkpointer** 是一个持久化层接口，这意味着历史的对话记录，可以被自动持久化到数据库（如 SQLite 或其他外部数据库）中。这使得即使应用程序重启或用户断开连接，对话历史也能被保存和恢复，从而实现“真正的多轮记忆”。

LangGraph 提供了多种 Checkpointer 以便应对不同的使用场景：

* MemorySaver 保存在内存，适用开发调试、单元测试；
* SqliteSaver 保存在本地的.db文件，轻量级应用、边缘计算适合单机部署；
* PostgresSaver 保存在 PostgreSQL，适合用在生产环境、多实例部署；
* RedisSaver 适合处理高频、短时会话；

LangGraph 通过 thread\_id 会话的唯一标识，结合 Checkpointer 就可以实现状态的隔离：

首先指定一个 指定一个 `thread_id`，所有相关的状态都会被保存到这个线程中。

```
config = {"configurable": {"thread_id": "conversation_1"}}
graph.invoke(input_data, config)
```

编译的时候传入 Checkpointer 即可。

```
# 创建 checkpointer
checkpointer = InMemorySaver()
# 编译图时传入 checkpointer
graph = builder.compile(checkpointer=checkpointer)
```

完整示例：

```
from langgr...