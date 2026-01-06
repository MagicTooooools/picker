---
title: claude-code-tools - A collection of productivity tools and plugins for Claude Code, Codex-CLI, and …
url: https://jimmysong.io/ai/claude-code-tools/
source: Jimmy Song – Jimmy Song's Blog
date: 2026-01-05
fetch_date: 2026-01-06T03:31:57.797646
---

# claude-code-tools - A collection of productivity tools and plugins for Claude Code, Codex-CLI, and …

[Read: From using AI to building AI systems](/blog/from-using-ai-to-building-ai-systems/), a defining note on what I’m exploring.

[![Brand Icon](/images/icons/jimmysong.svg)
AI NATIVE INFRA](/)

Search
⌘K

* [Blog](/blog/)

  ### Explore the Blog

  Dive into deep analyses on AI, cloud native, and industry trends.

  [View all](/blog/)

  + Technology
  + More

  #### Technology

  + [AI Engineering

    Artificial Intelligence engineering, large language models, and intelligent agent applications](/categories/ai-engineering/)
  + [Cloud Native

    Cloud native infrastructure including Kubernetes, Service Mesh, Envoy, and Gateway API](/categories/cloud-native/)
  + [Engineering Practice

    Engineering culture, development practices, processes, and tools](/categories/engineering-practice/)
  + [Open Source

    Open source ecosystem, community contributions, and industry insights](/categories/open-source/)

  #### More

  + [Review

    Reviews and commentary on books, movies, games, hardware, and incidents.](/categories/review/)
  + [Travel

    Travel journals and experiences from around the world.](/categories/travel/)
  + [Conference

    Recaps and insights from tech conferences like KubeCon.](/categories/conference/)
  + [Perspective

    Thoughts and analysis on economics, culture, politics, and society.](/categories/perspective/)
* [Explore](/)

  ### Discovery

  Explore content from multiple dimensions: by category, tag, search, and travel stories.

  + Feedback
  + More

  #### Feedback

  + [Contact

    Get in touch for sponsorships, partnerships, or inquiries.](/contact/)
  + [Business

    Partnerships, sponsorships, and in-depth industry analysis.](/business/)
  + [Announcements

    Stay up-to-date with the latest news and updates.](/notice/)

  #### More

  + [AI OSS Landscape

    AI resources for developers and tech enthusiasts.](/ai/)
  + [ArkSphere AI

    AI Native Infra, Agentic Runtime community.](/community/)
  + [Travel Stories

    Discover experiences and journeys beyond technology.](/travel/)
* [About](/about/)
* [中文
  中文](/zh/ai/claude-code-tools/)

#### Search

Search

Type:

All Types
Blog
AI
Book
News

**Keyboard Navigation:**

`Tab` next result
|
`Shift+Tab` previous result
|
`↑↓` navigate
|
`Enter` open link
|
`Esc` Close

[Home](/)[AI OSS Landscape](/ai/)
claude-code-tools

# claude-code-tools

A collection of productivity tools and plugins for Claude Code, Codex-CLI, and similar CLI coding agents.

pchalasani
·

Since 2025-07-30

Loading score...

Score:
--
/ 100

--
|
--

Score data unavailable

[GitHub](https://github.com/pchalasani/claude-code-tools) [Website](https://pypi.org/project/claude-code-tools/)

## Detailed Introduction

claude-code-tools, maintained by pchalasani, is a suite of productivity plugins and CLI utilities designed for Claude Code, Codex-CLI, and similar command-line coding agents. The project provides plugins such as `aichat`, `tmux-cli`, and `safety-hooks`, plus commands like `aichat`, `vault`, and `env-safe` to manage session continuation, terminal automation, secure env handling, and fast full-text session search for LLM-driven development workflows.

## Main Features

* Session continuation and trimming with `aichat`, including Rust/Tantivy full-text search and rollover strategies.
* Terminal automation via `tmux-cli`, reducing automation race conditions and improving agent reliability.
* Safety hooks and `env-safe` for preventing dangerous operations and inspecting `.env` files without exposing values.
* Hybrid architecture: Python CLI core, Rust search binary, and Node.js action menus for interactive workflows.

## Use Cases

Ideal for developers and teams who run parallel agent-driven tasks or need robust session management: resume long-running work without lossy compaction, search and recover past session context, automate interactive terminal workflows, and enforce safety controls in local and CI environments.

## Technical Characteristics

The project combines Python for CLI and orchestration, Rust (Tantivy) for high-performance full-text search and TUI, and Node.js for interactive menus. It emphasizes modular plugins, least-privilege tool permissions for subagents, and hook-based extensibility, and is distributed via PyPI with optional Rust/Cargo binaries for the search components.

Comments

Loading comments...
0

![claude-code-tools](https://opengraph.githubassets.com/1/pchalasani/claude-code-tools)

##### Score Breakdown

💻 CLI
🛠️ Dev Tools
🦾 Agents
🧰 Tool
🧲 Utility

##### Related Resources

###### [Skills](/ai/anthropic-skills/)

An open-source library from Anthropic for defining, sharing, and reusing …

Author：Anthropic

###### [AgentField](/ai/agentfield/)

Brings Kubernetes principles to agent runtimes, offering an identity-aware, …

Author：Agent Field

###### [CUGA](/ai/cuga-agent/)

An open-source generalist agent for enterprise, supporting web/API execution, …

Author：CUGA Project

Navigation

* [Advanced Search](/search/)
* [Slides Share](/slide/)
* [Blog Post Tags](/tags/)
* [AI OSS Landscape](/ai/)

News

* [KCD Beijing 2026](/notice/kcd-beijing-2026/)
* [ArkSphere Community Launch](/notice/announcement-arksphere-community/)
* [New Liquid Glass Theme](/notice/liquid-glass-theme/)
* [Website Update](/notice/site-major-update-202506/)

Featured

* [Envoy Fundamentals](https://academy.tetrate.io/courses/envoy-fundamentals)
* [Istio Fundamentals](https://academy.tetrate.io/courses/istio-fundamentals)
* [RAG Chatbot](https://github.com/rootsongjc/rag-chatbot)
* [Istio Multi-Cluster](https://github.com/rootsongjc/istio-multi-cluster)

About

* [Jimmy](/about/)
* [Contact](/contact/)
* [Business](/business/)
* [Community](/community/)

Powered By

* [Cloudflare](https://cloudflare.com)
* [Hugo](https://gohugo.io)
* [VS Code](https://code.visualstudio.com)
* [GitHub](https://github.com/rootsongjc)

Follow

* [X(Twitter)](https://x.com/jimmysongio)
* ![footer image](/images/jimmysong-x-qr-code.png)

© 2017-2026 Jimmy Song All Right Reserved · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)