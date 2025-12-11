---
title: CNCF in the AI Native Era? The Agentic AI Foundation Is Officially Established
url: https://jimmysong.io/blog/agentic-ai-foundation-cncf-era/
source: Jimmy Song – Jimmy Song's Blog
date: 2025-12-10
fetch_date: 2025-12-11T03:27:55.848797
---

# CNCF in the AI Native Era? The Agentic AI Foundation Is Officially Established

[ArkSphere Community](https://arksphere.dev/): AI native runtime, infrastructure, and open source.

[JIMMY SONG](/)

Search
⌘K

* [Blog](/blog/)

  ### Explore the Blog

  Dive into deep analyses on AI, cloud native and industry trends.

  [View all articles](/blog/)

  + Columns
  + Others

  #### Columns

  + [AI Engineering

    Artificial Intelligence engineering, large language models, and intelligent agent applications](/categories/ai-engineering/)
  + [Cloud Native

    Cloud native infrastructure including Kubernetes, Service Mesh, Envoy, and Gateway API](/categories/cloud-native/)
  + [Engineering Practice

    Engineering culture, development practices, processes, and tools](/categories/engineering-practice/)
  + [Open Source

    Open source ecosystem, community contributions, and industry insights](/categories/open-source/)

  #### Others

  + [Archive

    Archived content (travel, reviews, personal updates, etc.)](/categories/archive/)
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

  + [AI Resources

    AI resources for developers and tech enthusiasts.](/ai/)
  + [ArkSphere AI

    AI Native Infra, Agentic Runtime community.](/community/)
  + [Travel Stories

    Discover experiences and journeys beyond technology.](/travel/)
* [About](/about/)
* [中文
  中文](/zh/blog/agentic-ai-foundation-cncf-era/)

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

[Home](/)[Blog](/blog/)[AI Engineering](/categories/ai-engineering/)
Agentic AI Foundation and AI Native Standardization

# CNCF in the AI Native Era? The Agentic AI Foundation Is Officially Established

An analysis of the background, strategic urgency, differences and division of labor between Agentic AI Foundation (AAIF) and CNCF/CNAI, and its significance for the AI Native era.

Dec 10, 2025
•

[AI Engineering](/categories/ai-engineering)
•

4 Minute
•

986 words

Table of content

* [Cloud Native Problems Are Solved, AI Native Problems Are Just Beginning](#cloud-native-problems-are-solved-ai-native-problems-are-just-beginning)
* [AAIF’s Three Weapons: Protocol + Runtime + Development Standard](#aaifs-three-weapons-protocol--runtime--development-standard)
  + [① Anthropic’s Model Context Protocol (MCP)](#-anthropics-model-context-protocol-mcp)
  + [② Block’s Goose Framework](#-blocks-goose-framework)
  + [③ OpenAI’s AGENTS.md](#-openais-agentsmd)
* [Why Is AAIF in Such a Hurry? This Is a Race for Standards](#why-is-aaif-in-such-a-hurry-this-is-a-race-for-standards)
* [AAIF vs CNCF: Not Competition, But Two Pieces of the Puzzle](#aaif-vs-cncf-not-competition-but-two-pieces-of-the-puzzle)
* [The Real Challenge: Not Protocols, But Organizations and People](#the-real-challenge-not-protocols-but-organizations-and-people)
* [Summary: AAIF Is the Moment When Boundaries Are Drawn](#summary-aaif-is-the-moment-when-boundaries-are-drawn)
* [The ‘Three-Body’ Architecture of the AI Native Era](#the-three-body-architecture-of-the-ai-native-era)

> The standardization and open collaboration of the agent ecosystem is no longer a luxury, but the critical watershed for whether AI Native can be engineered and implemented.

* The establishment of
  [AAIF (Agentic AI Foundation)](https://aaif.io/)is the result of leading vendors staking out the “agent protocol layer” in advance.
* The real challenge is not technical, but how organizations transition from “human execution + AI assistance” to “agent execution + human supervision”.
* Successful agent adoption requires a phased adoption path, not just a bunch of protocols and demos.
* [CNCF](https://cncf.io/)and AAIF are complementary: CNCF manages “what infrastructure agents run on”, AAIF manages “how agents collaborate”. This matches the system I am building in
  [ArkSphere](https://arksphere.dev/).

## Cloud Native Problems Are Solved, AI Native Problems Are Just Beginning

Over the past decade, Cloud Native technologies like Kubernetes, Service Mesh, and microservices have standardized “how applications run in the cloud”.
But AI Native faces a completely different challenge:
**It’s not about “how to deploy a service”, but “how many behaviors in the system can be handed over to agents to execute themselves”.**

CNCF’s Cloud Native AI (CNAI) addresses infrastructure-level issues:
“How can model training/inference/RAG run at scale and securely on Kubernetes?”

But what AI Native truly lacks is another layer:
**How do agents collaborate, access tools, get governed, and audited?**

This is exactly the gap AAIF aims to fill.

## AAIF’s Three Weapons: Protocol + Runtime + Development Standard

AAIF hosts three core technologies contributed by its founding members:

### ① Anthropic’s Model Context Protocol (MCP)

<https://github.com/modelcontextprotocol>

A “system call interface for agents”:

* Unified definition for how agents access databases, APIs, files, and external tools.
* Designed to be more like an AI version of gRPC + OAuth.
* Already integrated by Claude, Cursor, ChatGPT, VS Code, Microsoft Copilot, Gemini, and others.

It may not be the flashiest technology, but it could become the plumbing for the entire Agentic ecosystem.

### ② Block’s Goose Framework

<https://github.com/block/goose>

Reference runtime for MCP:

* Local-first, composable agent workflow engine.
* Enables enterprises to pilot agents in small scopes without betting on a specific vendor.
* Serves as an engineering template for protocol implementation.

### ③ OpenAI’s AGENTS.md

[https://agents.md](https://agents.md/)

A simple but effective standard:

* Place an AGENTS.md file in the project repository.
* Clearly document build steps, testing, constraints, and context rules.
* Any agent that understands AGENTS.md can operate the codebase using the same instructions.

This makes agent behavior more predictable and auditable.

## Why Is AAIF in Such a Hurry? This Is a Race for Standards

Let’s compare with history:

* Kubernetes’ predecessor Borg ran internally at Google for over a decade; K8s was open sourced and donated to CNCF two years later.
* PyTorch joined the Linux Foundation six years after its release.
* MCP was donated to AAIF just **over one year** after its launch.

AAIF is not about “mature technology entering a foundation”, but **staking out the key position early**.

The reasons are practical:

1. **Prevent agent ecosystem fragmentation**
   Today, there are many competing “tool invocation protocols”, which could become incompatible silos in three years.
2. **Protocol layer is easier to reach global consensus than model layer**
   Model competition is inevitable, but protocols can be standardized, open sourced, and avoid vendor lock-in.
3. **A necessary move in global tech competition**
   Putting the foundational standards for Agentic AI into the Linux Foundation is both a gesture of cooperation and a strategic move.

## AAIF vs CNCF: Not Competition, But Two Pieces of the Puzzle

CNCF’s role:

> “What infrastructure do agent workloads run on?”
> Kubernetes, Service Mesh, observability, AI Gateway, RAG Infra—all at this layer.

AAIF’s role:

> “How do agents collaborate, invoke tools, and get governed?”
> Protocols, runtimes, and behavioral standards—all at this layer.

Analogy:

| Domain | Responsibilities |
| --- | --- |
| **AAIF** | Semantic and collaboration layer of Agentic Runtime |
| **CNCF/CNAI** | Resource and execution layer of AI Native Infra |

Table 1: AAIF vs CNCF Comparison

This matches the upper semantic and lower infrastructure layers ...