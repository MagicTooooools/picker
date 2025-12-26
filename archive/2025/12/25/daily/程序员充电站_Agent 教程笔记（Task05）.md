---
title: Agent 教程笔记（Task05）
url: https://itcharge.cn/tech/llm-dev/agent-tutorial-05/
source: 程序员充电站
date: 2025-12-25
fetch_date: 2025-12-26T03:27:05.587892
---

# Agent 教程笔记（Task05）

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

# Agent 教程笔记（Task05）

[2025-12-25](https://itcharge.cn/tech/llm-dev/agent-tutorial-05/)

3.299k 字  |  11 分钟

## 6. MCP 协议

---

### 6.1 MCP 协议核心概念

**MCP（Model Context Protocol）**：由 Anthropic 提出的标准化协议，用于智能体与外部工具/资源的通信。

#### 6.1.1 设计理念

* **统一接口**：像 USB-C 统一设备连接一样，MCP 统一了智能体访问各种服务的方式
* **上下文共享**：不仅是 RPC 调用，更重要的是共享丰富的上下文信息（代码结构、依赖关系、历史记录等）
* **模型无关**：无论使用 Claude、GPT 还是其他模型，只要支持 MCP，就能访问相同的工具

#### 6.1.2 三层架构

* **Host（宿主层）**：用户直接交互的界面（如 Claude Desktop），管理对话流程
* **Client（客户端层）**：负责与 MCP Server 建立连接，发送请求并接收响应
* **Server（服务器层）**：执行实际功能操作（文件系统、数据库、API 等）

**工作流程**：用户问题 → Host → LLM 分析 → 需要工具 → MCP Client → MCP Server → 执行操作 → 返回结果 → LLM 生成回答

#### 6.1.3 核心能力

MCP 提供三大核心能力：

* **Tools（工具）**：主动执行操作（如读取文件、执行计算）
* **Resources（资源）**：被动提供数据（如文件内容、数据库记录）
* **Prompts（提示模板）**：提供预定义的提示模板（如代码审查模板）

### 6.2 MCP vs Function Calling

**Function Calling**：LLM 的内在能力，体现模型的智能（理解何时调用、如何生成参数）

**MCP**：基础设施协议，解决工程层面的工具连接问题（标准化描述和调用方式）

**关系**：互补而非竞争。Function Calling 是"如何打电话"的技能，MCP 是"电话通信标准"。

**对比优势**：

* Function Calling：需要为每个 LLM 提供商定义不同格式，自己实现工具函数，处理不同响应格式
* MCP：统一的调用方式，社区提供现成服务器，任何支持 MCP 的模型都能使用

### 6.3 MCP 客户端使用

#### 6.3.1 连接服务器

```
from hello_agents.protocols import MCPClient
import asyncio

async def connect():
    # Stdio 模式：连接本地进程
    client = MCPClient([
        "npx", "-y",
        "@modelcontextprotocol/server-filesystem",
        "."  # 根目录
    ])

    async with client:
        tools = await client.list_tools()
        print(f"可用工具: {[t['name'] for t in tools]}")
```

#### 6.3.2 发现和调用工具

```
# 1. 发现可用工具
tools = await client.list_tools()

# 2. 调用工具
result = await client.call_tool("read_file", {"path": "README.md"})

# 3. 访问资源
resources = await client.list_resources()
content = await client.read_resource("file:///path/to/resource")

# 4. 使用提示模板
prompts = await client.list_prompts()
prompt = await client.get_prompt("code_review", {"language": "python"})
```

#### 6.3.3 传输方式

MCP 支持多种传输方式（传输层无关性）：

* **Memory Transport**：内存传输，用于测试和快速原型
* **Stdio Transport**：标准输入输出，本地开发最常用
* **HTTP/SSE/StreamableHTTP Transport**：远程服务，生产环境

### 6.4 在智能体中使用 MCP

#### 6.4.1 MCPTool 自动展开机制

`MCPTool` 会自动将 MCP 服务器提供的所有工具展开为独立工具：

```
from hello_agents import SimpleAgent, HelloAgentsLLM
from hello_agents.tools import MCPTool

agent = SimpleAgent(name="助手", llm=HelloAgentsLLM())

# 添加 MCP 工具（自动展开）
mcp_tool = MCPTool(
    name="calculator",
    server_command=["python", "calculator_server.py"]
)
agent.add_tool(mcp_tool)

# 智能体可直接使用展开后的工具
response = agent.run("计算 25 乘以 16")
```

**工作原理**：

1. MCPTool 连接到服务器，发现所有工具
2. 为每个工具创建包装器（如 `calculator_add`、`calculator_multiply`）
3. 注册到 Agent 的工具注册表
4. Agent 调用时自动转换为 MCP 格式并执行

#### 6.4.2 多服务器集成

```
# 文件系统服务器
fs_tool = MCPTool(
    name="fs",
    server_command=["npx", "-y", "@modelcontextprotocol/server-filesystem", "."]
)

# GitHub 服务器
github_tool = MCPTool(
    name="gh",
    server_command=["npx", "-y", "@modelcontextprotocol/server-github"]
)

agent.add_tool(fs_tool)
agent.add_tool(github_tool)
```

**注意**：多个 MCP 服务器时，必须为每个 `MCPTool` 指定不同的 `name`，避免工具名冲突。

### 6.5 MCP 社区生态

**三大资源库**：

1. **Awesome MCP Servers**：社区维护的精选列表
2. **MCP Servers Website**：官方目录网站
3. **Official MCP Servers**：Anthropic 官方维护

**常用官方服务器**：

* `@modelcontextprotocol/server-filesystem`：文件系统操作
* `@modelcontextprotocol/server-github`：GitHub API 集成
* `@modelcontextprotocol/server-postgres`：PostgreSQL 数据库

**社区热门服务器**：

* `@playwright/mcp`：网页自动化测试
* `@modelcontextprotocol/server-slack`：Slack 集成
* `@modelcontextprotocol/server-google-drive`：Google Drive 访问

### 6.6 构建自定义 MCP 服务器

#### 6.6.1 为什么需要自定义服务器

* 封装业务逻辑：将企业内部流程标准化
* 访问私有数据：安全可控的接口
* 性能优化：针对特定场景深度优化
* 功能定制：实现标准服务未提供的功能

#### 6.6.2 基本实现

```
from hello_agents.protocols import MCPServer

# 创建服务器
server = MCPServer(name="my-server", description="自定义服务器")

# 定义工具函数
def my_tool(param: str) -> str:
    """工具描述"""
    # 实现逻辑
    return result

# 注册工具
server.add_tool(my_tool)

# 运行服务器
if __name__ == "__main__":
    server.run()
```

#### 6.6.3 发布到 Smithery

**准备文件**：

* `smithery.yaml`：Smithery 配置文件（必需）
* `pyproject.toml`：Python 项目配置（必需）
* `Dockerfile`：Docker 构建配置（推荐）
* `README.md`：项目说明文档

**发布流程**：

1. 在 GitHub 创建仓库
2. 访问 [smithery.ai](https://smithery.ai/)
3. 使用 GitHub 账号登录
4. 点击 "Publish Server"，输入仓库 URL
5. 等待审核和发布

**使用已发布的服务器**：

```
weather_tool = MCPTool(
    server_command=["smithery", "run", "weather-mcp-server"]
)
```

赏

* **本文作者：**
  程序员充电站
* **本文链接：**
  https://itcharge.cn/tech/llm-dev/agent-tutorial-05/
* **版权声明：**
  本站所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) 许可协议。

* [大模型应用开发](https://itcharge.cn/category/tech/llm-dev/)
* [技术](https://itcharge.cn/category/tech/)

# 评论（没有评论）

![](https://itcharge.cn/wp-content/themes/Sapphire/assets/img/head.jpg)

# [程序员充电站](/)

高效率编程，慢节奏生活。

[83
文 章](/archive)

[18
分 类](/category)

[21
标 签](/tag)

# 最新文章

* [2024 年目标清单](https://itcharge.cn/life/learning/2024-flags/)
* [Agent 教程笔记（Task06）](https://itcharge.cn/tech/llm-dev/agent-tutorial-06/)
* [Agent 教程笔记（Task05）](https://itcharge.cn/tech/llm-dev/agent-tutorial-05/)
* [RAG 教程笔记（Task04）](https://itcharge.cn/tech/llm-dev/rag-tutorial-04/)
* [RAG 教程笔记（Task03）](https://itcharge.cn/tech/llm-dev/rag-tutorial-03/)

# 热门标签

* [技术](https://itcharge.cn/tag/%E6%8A%80%E6%9C%AF/)
* [iOS 开发](https://itcharge.cn/tag/ios-dev/)
* [北航考研](https://itcharge.cn/tag/%E5%8C%97%E8%88%AA%E8%80%83%E7%A0%94/)
* [生活](https://itcharge.cn/tag/life/)
* [数据结构](https://itcharge.cn/tag/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/)
* [C 语言](https://itcharge.cn/tag/c-%E8%AF%AD%E8%A8%80/)
* [学习成长](https://itcharge.cn/tag/learning/)
* [高效率](https://itcharge.cn/tag/%E9%AB%98%E6%95%88%E7%8E%87/)
* [macOS 技巧](https://itcharge.cn/tag/macos-%E6%8A%80%E5%B7%A7/)
* [读书笔记](https://itcharge.cn/tag/%E8%AF%BB%E4%B9%A6%E7%AC%94%E8%AE%B0/)
* [考研](https://itcharge.cn/tag/%E8%80%83%E7%A0%94/)
* [工具思维](https://itcharge.cn/tag/%E5%B7%A5%E5%85%B7%E6%80%9D%E7%BB%B4/)
* [慢生活](https://itcharge.cn/tag/%E6%85%A2%E7%94%9F%E6%B4%BB/)
* [Hexo 博客](https://itcharge.cn/tag/hexo-%E5%8D%9A%E5%AE%A2/)
* [WordPress 博客](https://itcharge.cn/tag/wordpress-%E5%8D%9A%E5%AE%A2/)
* [诗意人生](https://itcharge.cn/tag/romantic-life/)
* [厨艺美食](https://itcharge.cn/tag/cooking/)
* [LeetCode](https://itcharge.cn/tag/leetcode/)
* [算法](https://itcharge.cn/tag/%E7%AE%97%E6%B3%95/)
* [读书随笔](https://itcharge.cn/tag/book-notes/)
* [算法 & 数据结构](https://itcharge.cn/tag/%E7%AE%97%E6%B3%95-%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/)

# 联系方式

# 目 录

谢谢你请我喝咖啡~

![扫码支持](https://itcharge.cn/wp-content/uploads/wxpay.png "扫一扫")
![扫码支持](https://itcharge.cn/wp-content/uploads/alipay.png "扫一扫")

扫码打赏，支持一下

![微信](https://itcharge.cn/wp-content/themes/Sapphire/assets/img/praise/wechat_pay.svg)
微信支付

![支付宝](https://itcharge.cn/wp-content/themes/Sapphire/assets/img/praise/ali_pay.svg)
支付宝

打开微信扫一扫，即可进行扫码打赏哦

本站总访问量  次
|
本站总访客数  人

©
2016-2025 程序员充电站 | [京ICP备18044298号-2](https://beian.miit.gov.cn/) | Theme [Sapphire](https://itcharge.cn/wordpress-theme-sapphire/) by ITCharge