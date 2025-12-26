---
title: ReAct Prompting
url: https://www.ttalk.im/s/kQKMTK
source: The Talk
date: 2025-12-25
fetch_date: 2025-12-26T03:26:44.920908
---

# ReAct Prompting

# ReAct Prompting

LLM

## 什么是 ReAct？

ReAct = Reasoning（推理）+ Acting（行动），由 Yao 等人于 2022 年提出。核心思想是让 LLM 以交错方式生成推理痕迹和执行任务行动。

## 工作流程

### ReAct 采用三步循环：

1. Thought（思考） - 模型规划下一步行动

2. Action（行动） - 与外部资源交互（如搜索引擎、知识库）

3. Observation（观察） - 获取反馈，调整推理方向

## 与传统 CoT 的区别

| 方法 | 特点 |  |
| --- | --- | --- |
| Chain-of-Thought | 仅依赖内部知识，容易产生幻觉 |  |
| ReAct | 能访问外部信息源，减少事实错误 |  |

### 优势

* 在知识密集型任务（如问答、事实验证）表现优异
* 推理过程可解释、可追踪
* 与 CoT 结合使用效果最佳

### 局限性

* 依赖外部搜索结果的质量
* 检索信息不足时，模型难以恢复推理
* 某些任务仍不如人类专家

## 一句话总结：

ReAct 通过让 LLM "边思考边行动"，克服了纯推理方法的知识局限，是构建 AI Agent 的重要技术基础。

扫描二维码分享此链接

![QR Code](data:image/png;base64...)

[🔗 访问目标链接](https://www.promptingguide.ai/techniques/react)

Short code: kQKMTK • Powered by Owl