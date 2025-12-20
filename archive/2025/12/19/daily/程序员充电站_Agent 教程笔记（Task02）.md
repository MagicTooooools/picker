---
title: Agent 教程笔记（Task02）
url: https://itcharge.cn/tech/llm-dev/agent-tutorial-02/
source: 程序员充电站
date: 2025-12-19
fetch_date: 2025-12-20T03:19:59.600666
---

# Agent 教程笔记（Task02）

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

# Agent 教程笔记（Task02）

[2025-12-19](https://itcharge.cn/tech/llm-dev/agent-tutorial-02/)

5.512k 字  |  19 分钟

## 2. 构建 Agent 框架

### 2.1 框架设计理念

**核心原则**：分层解耦、职责单一、接口统一

**设计理念**：

* **轻量级与教学友好**：极简依赖，代码清晰可读
* **基于标准 API**：基于 OpenAI API 标准，兼容主流 LLM 提供商
* **渐进式学习**：每章代码可 pip 下载，逐步迭代
* **万物皆为工具**：Memory、RAG、MCP 等统一抽象为工具

**自建框架的价值**：

* 深度理解 Agent 工作原理
* 获得完全控制权，精确调优
* 培养系统设计能力

### 2.2 HelloAgentsLLM 扩展

**多提供商支持**：通过继承 `HelloAgentsLLM`，重写 `__init__` 方法扩展新供应商，不修改库源码

**本地模型调用**：

* **VLLM**：高性能推理，服务地址 `http://localhost:8000/v1`
* **Ollama**：简化部署，服务地址 `http://localhost:11434/v1`
* 两者都兼容 OpenAI API，可无缝接入

**自动检测机制**（优先级）：

1. 特定服务商环境变量（最高）
2. 根据 `base_url` 判断（域名/端口匹配）
3. API 密钥格式分析（辅助）

### 2.3 框架接口实现

**Message 类**：统一消息格式，使用 `Literal` 限制 role，`to_dict()` 兼容 OpenAI API

**Config 类**：中心化配置管理，支持环境变量覆盖，合理默认值

**Agent 抽象基类**：

* 使用 `ABC` 和 `@abstractmethod` 强制子类实现 `run` 方法
* 提供统一的历史记录管理
* 定义所有智能体的通用行为

### 2.4 Agent 范式框架化实现

**SimpleAgent**：基础对话 Agent，支持可选工具调用、流式响应

**ReActAgent**：推理与行动结合，利用统一 `ToolRegistry`，提示词优化

**ReflectionAgent**：三阶段（Initial → Reflect → Refine），通用化设计，支持自定义提示词

**PlanAndSolveAgent**：两阶段（Planner → Executor），强制 Python 列表格式输出，完整异常处理

**FunctionCallAgent**：基于 OpenAI 原生函数调用，更强鲁棒性

### 2.5 工具系统

**Tool 基类**：统一 `run` 接口，自描述能力（`get_parameters`），内省机制

**ToolRegistry**：

* 两种注册方式：Tool 对象注册（复杂工具）、函数直接注册（简单工具）
* 核心功能：注册、发现、执行、描述生成

**自定义工具开发**：编写函数 → 注册到 Registry → 与 Agent 集成

**多源搜索工具**：智能后端选择（Tavily 优先 → SerpApi 降级），统一格式化

**高级特性**：

* **工具链**：支持多工具顺序执行，变量替换，步骤间结果引用
* ## **异步执行**：使用线程池和 `asyncio` 并行执行，提升性能

## 3. 课后习题

### 习题1：框架设计分析

**1.1 主流框架局限性对开发效率的影响**

以 LangChain 为例：

* **过度抽象**：需要先理解 Chain、Agent、Tool、Memory 等十几个概念，学习曲线陡峭，简单任务也需要大量配置
* **快速迭代**：API 频繁变更导致代码需要频繁适配，维护成本高（最新 1.0 版本进行了重构）
* **黑盒化**：遇到问题时难以定位，只能依赖文档和社区
* **依赖复杂**：依赖包体积大，与其他项目可能出现冲突

**1.2 「万物皆为工具」设计理念**

**优势**：

* 消除抽象层，回归核心逻辑「智能体调用工具」
* 统一接口，降低学习成本
* 易于扩展：Memory、RAG 等只需实现 Tool 接口即可集成

**局限性**：

* 工具间的依赖关系管理可能变得复杂

**1.3 框架化改进**

* **代码组织**：从独立脚本到统一框架结构，便于维护
* **提示词工程**：从特定任务到通用化模板，可配置可扩展
* **工具集成**：从硬编码到统一 ToolRegistry，易于管理
* **配置管理**：从分散到集中 Config 类，支持环境变量

**设计原则优先级**：

1. 接口统一性（所有 Agent 遵循相同接口）
2. 职责单一性（每个类方法职责明确）
3. 可扩展性（通过继承和组合扩展）
4. 教学友好性（代码清晰可读）

### 习题 2：LLM 扩展实践

**2.1 添加新供应商支持**

```
class MyGeminiLLM(HelloAgentsLLM):
    def __init__(self, provider="auto", **kwargs):
        if provider == "gemini":
            self.provider = "gemini"
            self.api_key = kwargs.get('api_key') or os.getenv("GEMINI_API_KEY")
            if not self.api_key:
                raise ValueError("Gemini API key not found")
            self.base_url = kwargs.get('base_url') or "https://generativelanguage.googleapis.com/v1beta"
            self.model = kwargs.get('model') or "gemini-pro"
            self._client = OpenAI(api_key=self.api_key, base_url=self.base_url)
        else:
            super().__init__(provider=provider, **kwargs)
```

**2.2 自动检测优先级分析**

如果同时设置 `OPENAI_API_KEY` 和 `LLM_BASE_URL="http://localhost:11434/v1"`：

* 框架会优先检测到 `OPENAI_API_KEY`，选择「openai」提供商
* 这种设计 **不够合理**，因为用户明确设置了本地服务的 base\_url，应该优先使用本地服务
* **改进建议**：base\_url 的优先级应该高于通用 API 密钥，或者增加显式 provider 参数

**2.3 本地模型部署方案对比**

| 特性 | VLLM | SGLang | Ollama |
| --- | --- | --- | --- |
| 易用性 | 中等 | 中等 | 高（最简单） |
| 资源占用 | 高 | 中等 | 低 |
| 推理速度 | 最快 | 快 | 中等 |
| 推理精度 | 高 | 高 | 高 |
| 适用场景 | 生产环境高并发 | 需要灵活控制 | 快速原型开发 |

### 习题 3：框架接口设计分析

**3.1 Pydantic BaseModel 的优势**

* **类型验证**：自动验证数据类型，减少运行时错误
* **数据序列化**：自动支持 JSON 序列化/反序列化
* **IDE 支持**：更好的代码补全和类型提示
* **文档生成**：自动生成 API 文档

**3.2 设计模式**

模板方法模式（Template Method Pattern）

* **好处**：保证所有 Agent 都有统一的调用接口，同时允许子类自定义实现

**3.3 单例模式**

**单例模式**：确保一个类只有一个实例，并提供全局访问点

**为什么配置管理需要单例**：

* 配置应该是全局唯一的，避免多个实例导致配置不一致
* 便于全局访问，不需要传递配置对象
* 节省内存，避免重复创建

**不使用单例会导致的问题**：

* 多个配置实例可能导致配置不一致
* 难以实现配置的全局更新和同步

### 习题 4：Agent 范式实现

**4.1 ReActAgent 改进点**

1. **统一工具接口**：使用 ToolRegistry 替代硬编码工具字典，便于扩展
2. **提示词模板化**：可配置的提示词模板，支持自定义
3. **异常处理完善**：增加完整的异常处理和错误提示

**4.2 ReflectionAgent 质量评分机制**

```
def run(self, input_text: str, quality_threshold: float = 0.8, **kwargs) -> str:
    # 初始执行
    current_output = self._initial_execute(input_text)

    for iteration in range(self.max_iterations):
        # 反思
        feedback = self._reflect(input_text, current_output)

        # 质量评分
        score = self._evaluate_quality(input_text, current_output, feedback)
        if score >= quality_threshold:
            return current_output  # 提前终止

        # 优化
        current_output = self._refine(input_text, current_output, feedback)

    return current_output

def _evaluate_quality(self, task: str, output: str, feedback: str) -> float:
    """让 LLM 对当前输出打分（0-1）"""
    prompt = f"请对以下输出质量打分（0-1）：\n任务：{task}\n输出：{output}\n反馈：{feedback}\n只返回 0-1 之间的数字"
    score_text = self.llm.invoke([{"role": "user", "content": prompt}])
    return float(score_text.strip())
```

**4.3 Tree-of-Thought Agent 设计**

```
class TreeOfThoughtAgent(Agent):
    def run(self, input_text: str, branching_factor: int = 3, **kwargs) -> str:
        # 生成多个思考路径
        thoughts = self._generate_thoughts(input_text, branching_factor)

        # 评估每个路径
        scored_thoughts = [(t, self._evaluate_thought(t)) for t in thoughts]

        # 选择最优路径
        best_thought = max(scored_thoughts, key=lambda x: x[1])[0]

        # 继续扩展最优路径
        return self._expand_path(best_thought, input_text)
```

### 习题 5：工具系统设计

**5.1 统一接口的必要性**

**原因**：

* 保证框架一致性，所有工具都能以相同方式调用
* 简化 Agent 的实现，不需要为每种工具写特殊逻辑
* 便于工具管理和发现

**返回多个值的设计**：

```
class SearchTool(Tool):
    def run(self, parameters: Dict[str, Any]) -> str:
        results = self._search(parameters['query'])
        # 返回结构化 JSON 字符串
        return json.dumps({
            'titles': [r.title for r in results],
            'summaries': [r.summary for r in results],
            'urls': [r.url for r in results]
        })
```

**5.2 工具链应用场景**

**场景**：智能报告生成

* 工具 1：搜索工具（获取最新信息）
* 工具 2：计算工具（处理数据）
* 工具 3：文本生成工具（生成报告）

**执行流程图**：

```
用户输入 → 搜索工具 → 计算工具 → 文本生成工具 → 最终报告
          ↓          ↓          ↓
        搜索结果    计算结果    报告草稿
```

**5.3 并行执行的优势场景**

* **独立工具调用**：多个工具之间无依赖关系
* **IO 密集型操作**：如网络请求、文件读取
* **批量处理**：需要处理大量相似任务

**不适用场景**：

* 工具间有依赖关系
* CPU 密集型计算（Python GIL 限制）
* 需要严格顺序执行的任务

### 习题 6：框架扩展设计

**6.1 流式输出功能**

**实现方案**：

* 修改 `Agent.run()` 方法，增加 `stream=True` 参数
* 在 `SimpleAgent` 中，调用 `llm.stream_invoke()` 替代 `llm.invoke()`
* 使用生成器模式，`yield` 每个文本块
* 需要修改的类：`Agent` 基类（增加抽象方法 `stream_run`）、`SimpleAgent`（实现流式逻辑）

**6.2 多轮对话管理**

**设计**：

* 新增 `ConversationManager` 类，管理对话树结构
* 支持对话分支（用户可以选择不同的回复路径）
* 支持回溯（回到历史对话节点）
* 与 `Message` 系统集成：每个 Message 关联一个对话节点 ID

**新增类**：

```
class ConversationNode:
    message: Message
    parent: Optional['ConversationNode']
    children: List['ConversationNode']

class ConversationManager:
    current_node: ConversationNode
    root_node: ConversationNode

    def branch(self, new_message: Message) -> ConversationNode
    def backtrack(self, node_id: str) -> None
```

**6.3 插件系统架构**

**架构图**：

```
HelloAgents Core
    ↓
Plugin Interface（抽象接口）
    ↓
┌─────────────┬─────────────┬─────────────┐
│ Agent Plugin│ Tool Plugin  │ LLM Plugin  │...