---
title: Agent 教程整理
url: https://itcharge.cn/tech/llm-dev/agent-tutorial/
source: 程序员充电站
date: 2025-12-17
fetch_date: 2025-12-18T03:24:47.596970
---

# Agent 教程整理

# [![](https://itcharge.cn/wp-content/uploads/header-logo.png)](https://itcharge.cn/)

[Menu](#0)

* [技术](https://itcharge.cn/category/tech/)
  + [iOS 开发](https://itcharge.cn/category/tech/ios-dev/)
  + [算法 & 数据结构](https://itcharge.cn/category/tech/algorithm/)
  + [Hexo 博客](https://itcharge.cn/category/tech/hexo-blog/)
  + [WordPress 博客](https://itcharge.cn/category/tech/wordpress-blog/)
* [高效率](https://itcharge.cn/category/efficient/)
  + [macOS 技巧](https://itcharge.cn/category/efficient/macos-tip/)
  + [工具思维](https://itcharge.cn/category/efficient/tool-mind/)
* [慢生活](https://itcharge.cn/category/life/)
  + [读书笔记](https://itcharge.cn/category/life/book-notes/)
  + [学习成长](https://itcharge.cn/category/life/learning/)
  + [厨艺美食](https://itcharge.cn/category/life/cooking/)
  + [诗意人生](https://itcharge.cn/category/life/brilliant-life/)
  + [考研相关](https://itcharge.cn/category/life/graduate-test/)
* [关于我](https://itcharge.cn/life/about-me/)

# Agent 教程整理

[2025-12-18](https://itcharge.cn/tech/llm-dev/agent-tutorial/)

5.686k 字  |  19 分钟

## 1. Agent 三种经典范式：ReAct / Plan-and-Solve / Reflection

### 1.1 ReAct（Reasoning and Acting）

**核心思想：推理 + 行动交替进行。**

模型一边思考（Thought），一边调用工具或执行动作（Action），再根据观察结果（Observation）继续下一轮推理和行动。

#### 1.1.1 环境准备与基础工具定义

* **LLM 客户端 `HelloAgentsLLM`（关键要点）**

  + 封装统一接口：`think(messages: List[Dict], temperature=0) -> str`；
  + 内部负责：加载 `.env`、构造 `OpenAI` 客户端、处理流式响应（打印 + 累积拼接）、统一异常处理；
  + 好处：上层 Agent 只关心「传入 messages，拿到字符串」，降低对具体 SDK 的耦合。
* **工具系统 `ToolExecutor`（通用于 ReAct / Planner / Executor）**

  + 抽象一个通用工具注册与调用器：
  + `registerTool(name, description, func)`：登记工具元信息；
  + `getTool(name)`：按名称拿到实际可调用函数；
  + `getAvailableTools()`：拼成「name + description」列表，注入到 Prompt 里给 LLM 选择。
  + 设计要点：
  + 工具三元组：**名称 / 描述 / 执行逻辑**；描述要写给「模型」看，说明「何时该用我」；
  + 可以逐步扩展出：计算器、搜索、DB 查询、HTTP API 调用等统一管理的工具集。
* **搜索工具 `search`（示例）**

  + 使用 SerpApi/搜索引擎封装为一个工具函数：
  + 读取 `SERPAPI_API_KEY`；
  + 请求参数中固定 `engine=google, gl=cn, hl=zh-cn` 等；
  + 智能解析优先级：`answer_box_list` → `answer_box.answer` → `knowledge_graph.description` → `organic_results` 前三条摘要；
  + 给 LLM 一个清晰描述：**「当需要实时信息 / 事实 / 大模型知识外内容时，调用我」**。

#### 1.1.2 Prompt 典型结构

* **角色设定**：
  + 你是一个可以调用工具解决任务的智能 Agent。
* **输出格式约定**（关键）：
  + Thought: 解释你现在在想什么、下一步准备做什么；
  + Action: 要调用的工具名（例如 `search`、`calculator`、`get_user_info` 等）；
  + Action Input: 工具的输入参数；
  + Observation: 工具返回的结果；
  + 最终使用 Final Answer 给出答案。
* **Few-shot 示例**：
  + 至少给 1~2 个完整的「Thought → Action → Action Input → Observation → Final Answer」过程示例，帮助模型学会这种交替模式。

#### 1.1.3 ReActAgent 的核心循环实现

* **ReActAgent 的主循环（run）——逻辑拆分**

  1. **构造 Prompt**：把当前 `question / tools_desc / history` 填入模板；
  2. **调用 LLM**：`llm_client.think(messages)` 得到完整文本；
  3. **解析输出**：
     + `_parse_output(text)`：正则提取 `Thought:` 与 `Action:`；
     + `_parse_action(action_text)`：解析 `ToolName[tool_input]` 结构；
  4. **分支执行**：
     + 若 `Action` 为 `Finish[...]`：直接抽取最终答案、结束；
     + 否则根据 `tool_name` 用 `ToolExecutor.getTool()` 找到函数并执行，拿到 `Observation`；
  5. **更新历史**：`self.history.append("Action: ..."); self.history.append("Observation: ...")`；
  6. **安全终止**：到达 `max_steps` 未 `Finish` → 认为失败/异常，终止并返回 `None`。
* **ReAct 结合工具的典型示例**

  + 任务：「华为最新的手机是哪一款？主要卖点是什么？」
  + 步骤：
    1. Step1 Thought：意识到自身知识不够、需调用搜索 → `Action: Search[...]`；
    2. Search 工具返回搜索摘要（Observation）；
    3. Step2 Thought：阅读 Observation，综合提炼关键型号和卖点；
    4. `Action: Finish[...]` 输出结构化自然语言答案。

#### 1.1.4 ReAct 的特点、局限性与调试技巧

* **优点**

  + 强可解释：`Thought` 直接展示推理轨迹，方便 Debug；
  + 动态：可根据 Observation 调整后续动作，适合探索性任务；
  + 易组合工具：天然面向「多工具协同」。
* **局限**

  + 依赖 LLM 格式遵循与推理能力，输出一乱、正则就崩；
  + 多轮 LLM 调用 → 成本高、时延大；
  + 缺乏全局规划，容易「短视」或在局部打转。
* **调试技巧**

  + 打印「完整 Prompt」与「LLM 原始输出」；
  + 出错时先判断是：Prompt 不清晰 / 输出不规范 / 正则太脆弱；
  + 尝试加入 few-shot ReAct 例子，或改成更结构化的输出（如 JSON）再解析。

#### 1.1.5 适用场景

* **多步工具调用任务**：需要边查边想，比如「先查价格，再算总价，再生成报告」；
* **复杂信息整合**：需要从多个渠道汇总信息，再推理得出结论；
* **交互式问答 / 助手**：如搜索助手、数据分析助手等。

---

### 1.2 Plan-and-Solve（先规划再求解）

**核心思想：先生成整体计划（Plan），再按计划逐步执行（Solve）。**

相比 ReAct 更强调「全局规划 + 有计划地执行」，可以看作是宏观上的任务分解框架。

#### 1.2.1 整体流程（两阶段）

1. **Plan 阶段（规划阶段）**
   * 输入：用户的原始任务描述；
   * 要求模型输出一个结构化计划：
     + Step 1：要做什么，目标是什么；
     + Step 2：要做什么，依赖前一步的什么结果；
     + Step 3：……
   * 可以用列表、编号、甚至 JSON 来表达，每一步最好都包含「子目标 + 粗略方法」。
2. **Solve 阶段（执行阶段）**
   * 按照 Plan 中定义的每一步，依次或者并行执行：
     + 把当前 Step 作为「子任务」传给执行 Agent；
     + 子任务内部可以继续使用 ReAct 模式调用工具；
     + 每一步的中间结果要存储下来（context / memory），供后续步骤引用；
   * 最后汇总所有步骤的结果，生成最终答案或交付物。

#### 1.2.2 典型架构拆分

* **Planner Agent（规划代理）**
  + 负责：阅读用户需求 → 输出结构化计划；
  + 输出格式示例：
  + 自然语言分步；
  + 或 JSON：`[{step: 1, goal: "...", detail: "..."}, ...]`。
* **Executor / Worker Agents（执行代理）**
  + 负责：逐步执行每一个 Plan Step；
  + 内部可选：
  + 简单任务：直接一次调用模型 / 工具；
  + 复杂任务：再内部用 ReAct 或子计划。
* **Controller / Orchestrator（调度控制器）**
  + 管理整体流程：
  + 顺序 / 并行执行不同的 Step；
  + 错误重试、超时处理；
  + Step 之间的依赖关系与上下文传递。

#### 1.2.3 Plan-and-Solve 实战实现：Planner + Executor 双 Agent

* **核心思想回顾（工程视角）**

  + 使用两个角色：
  + `Planner`：专门负责「把问题拆成步骤」；
  + `Executor`：专门负责「按步骤执行」，可内部再次用 ReAct；
  + 好处：把「任务分解」与「任务执行」职责分离，便于调参和替换子模块。
* **Planner：用 LLM 输出「可解析」的计划**

  + 提示词核心：
  + 角色：「顶级 AI 规划专家」；
  + 输出形式：一个 Python 列表，包含每步的自然语言任务描述；
  + 要求用 `` `python … ``  `包裹，方便后续` ast.literal\_eval`。
  + 实现要点：
  + `plan(question)`：
    - 使用 `PLANNER_PROMPT_TEMPLATE` 填充问题；
    - 调用 `llm_client.think` 获取文本；
    - 从 `` `python `` `代码块中截取列表字符串，用` ast.literal\_eval `转为` list[str]`；
    - 出错时打印原始响应，返回空列表防御性退出。
* **Executor：按计划逐步执行并维护状态**

  + 提示词 `EXECUTOR_PROMPT_TEMPLATE` 核心字段：
  + 原始问题 `question`；
  + 完整计划 `plan`（可直接把 list 字符串化）；
  + 到目前为止的 `history`（前几步的「步骤 + 结果」）；
  + 当前要执行的 `current_step`。
  + `execute(question, plan)` 流程：
    1. 初始化 `history = ""`；
    2. 循环 `for i, step in enumerate(plan)`：
       - 用模板填充上下文 + 当前 step，调用 LLM；
       - 将「步骤 i+1 + 结果」追加到 `history`，供下一步使用；
    3. 最后一次响应作为最终答案返回。
* **PlanAndSolveAgent：组合层**

  + `run(question)`：
    1. 先 `self.planner.plan(question)` 得到计划；
    2. 若计划为空 → 直接终止；
    3. 否则 `self.executor.execute(question, plan)` 获取最终答案。
  + 与 ReAct 的对比要点：
  + ReAct：**在线、小步、紧耦合**（思考与工具交替）；
  + Plan-and-Solve：**先全局、后局部**，适合结构化强、路径相对清晰的问题。

#### 1.2.4 适用场景

* 复杂度高、步骤多、需要清晰分解的任务：
  + 如「撰写调研报告」「生成一个小型应用」「搭建数据分析流水线」；
* 需要提高「可解释性与可控性」的业务场景：
  + 可以对外展示计划，支持用户在执行前/中修改、介入。

---

### 1.3 Reflection（自反 / 反思式 Agent）

**核心思想：Agent 在完成任务后，对自己的过程进行复盘与反思，将经验沉淀下来，下次遇到相似任务时能少踩坑、做得更好。**

可以把它看作是：在 ReAct / Plan-and-Solve 之上，增加一层「持续学习与自我改进机制」。

#### 1.3.1 标准三阶段框架

1. **Task Execution（任务执行）**
   * 使用 ReAct 或 Plan-and-Solve 完成某个任务；
   * 同时记录执行日志：
     + 任务描述；
     + 中间推理（Thought）与工具调用（Action / Observation）；
     + 最终结果，以及任务是否成功（由规则或人工标注）。
2. **Reflection（反思阶段）**
   * 对执行日志做「总结 + 归因 + 提炼经验」：
   * 典型 Prompt 内容：
     + 输入：任务描述 + 关键过程摘要 + 成败情况；
     + 输出：
     + 哪些决策是正确且关键的？
     + 错误出在哪里，根本原因是什么？
     + 下次面对类似任务应遵循什么准则 / 步骤？
   * 结果通常是一段「经验总结」或「改进建议」文本。
3. **Memory / Policy Update（记忆与策略更新）**
   * 把反思结果写入某种「记忆库」：
     + 向量库：按任务语义索引经验；
     + 规则库：将高价值经验转为明确规则；
     + Prompt 片段：保存为可复用的 few-shot 示例或提示语。
   * 下次新任务开始前：
     + 根据当前任务语义，从记忆库检索相关经验；
     + 将这些经验注入到当前 Prompt（系统 / 上下文）中：
     + 例如：「历史类似任务的经验：…… 请在本次任务中遵循这些经验并避免过去错误。」

#### 1.3.2 实现关键点

* 需要定义统一的「任务执行日志格式」，便于事后传给模型做反思；
* 需要一个「相似任务检索机制」（向量数据库或关键词）；
* 需要设计「经验注入策略」：
  + 注入到 System Prompt；
  + 或作为 Few-shot 示例；
  + 或转化为显式规则供控制器使用。
* 成本控制：
  + 不必对所有任务做反思，可针对失败任务 / 高价值任务；
  + 或定期批量对采样任务进行反思。

#### 1.3.3 Reflection 实战实现：Memory + 三类 Prompt

* **记忆模块 `Memory`（短期经验轨迹）**

  + 作用：记录每轮「执行（execution）」和「反思（reflection）」的文本；
  + 接口：
  + `add_record(type, content)`：type ∈ {'execution', 'reflection'}；
  + `get_last_execution()`：获取最近一次代码 / 输出；
  + `get_trajectory()`：把所有记录转成带标题的长文本，适合塞进 Prompt 做「全过程回顾」。
* **三类 Prompt 角色分工**

  + `INITIAL_PROMPT_TEMPLATE`（执行者 / 程序员）：
  + 输入：任务描述；
  + 输出：第一版代码（含函数签名、docstring、PEP8）。
  + `REFLECT_PROMPT_TEMPLATE`（严格评审员 / 算法专家）：
  + 输入：任务描述 + 代码；
  + 聚焦：时间复杂度、算法瓶颈、是否存在更优算法；
  + 输出：自然语言反馈，指出具体问题与优化方向。
  + `REFINE_PROMPT_TEMPLATE`（根据反馈重写代码的程序员）：
  + 输入：任务描述 + 上一版代码 + 评审反馈；
  + 输出：优化后的新版本代码。
* **ReflectionAgent：执行–反思–优化循环**

  + 初始化：`max_iterations` 控制最多迭代轮数；
  + `run(task)`：
    1. **初始执行**：用 INITIAL 模板生成第一版代码，记为一条 `execution`；
    2. 循环 `i in [1..max_iter...