---
title: Decoding Goose: Why It Joined AAIF and What This Means for Agentic Runtime
url: https://jimmysong.io/blog/goose-aaif-agentic-runtime/
source: Jimmy Song – Jimmy Song's Blog
date: 2025-12-12
fetch_date: 2025-12-13T03:20:41.462033
---

# Decoding Goose: Why It Joined AAIF and What This Means for Agentic Runtime

Join the
[ArkSphere community](https://arksphere.dev/)to build the Agentic Runtime together.

[![Brand Icon](/images/logo.svg)
AI NATIVE INFRA](/)

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
  中文](/zh/blog/goose-aaif-agentic-runtime/)

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
Decoding Goose and AAIF

# Decoding Goose: Why It Joined AAIF and What This Means for Agentic Runtime

An analysis of Block’s Goose project, why it became one of the first Agentic AI Foundation (AAIF) projects, and what this means for Agentic Runtime and the evolution of AI-Native infrastructure.

Dec 12, 2025
•

[AI Engineering](/categories/ai-engineering)
•

5 Minute
•

1076 words

Table of content

* [Goose’s True Positioning: It’s Not an IDE or a Chatbot](#gooses-true-positioning-its-not-an-ide-or-a-chatbot)
* [Block’s AI Transformation Started with Organization, Not Tools](#blocks-ai-transformation-started-with-organization-not-tools)
* [Why Did Goose Enter AAIF? Not Because It’s “Technically Strongest”](#why-did-goose-enter-aaif-not-because-its-technically-strongest)
* [Goose’s Value Lies Not in Today, But in 2–3 Years](#gooses-value-lies-not-in-today-but-in-23-years)
* [AAIF Is an Attempt at Infrastructure-Level Consensus](#aaif-is-an-attempt-at-infrastructure-level-consensus)
* [Summary](#summary)

> Goose is not a project that excites you at first glance in this wave of Agent innovation, but its entry into AAIF signals a deeper shift in how we think about Agentic Runtime and AI-Native infrastructure.

At first glance,
[Goose](https://github.com/block/goose)is not a project that immediately excites people.

![Figure 1: Goose App UI](https://assets.jimmysong.io/images/blog/goose-aaif-agentic-runtime/goose.webp)

Figure 1: Goose App UI

It doesn’t have flashy demos, nor does it showcase overwhelming multimodal capabilities, and it certainly doesn’t look like an AI product aimed at consumers. Yet, this seemingly “plain” project became one of the first donations to the Agentic AI Foundation (AAIF), standing alongside Anthropic’s MCP and OpenAI’s AGENTS.md.

▸
About Goose’s Contributor: Block

[Block](https://block.xyz)(formerly Square) was founded in 2009 by Jack Dorsey and has over 10,000 employees and thousands of engineers. Although often categorized as a fintech company, Block has always been driven by engineering and systems capabilities, with a complex tech stack and high automation demands.

In its recent AI transformation, Block focused on **automation at the engineering execution layer** rather than simply introducing point tools. Goose was born in this context as an internal engineering product, connecting large models with real systems (code, tests, UI, internal tools), and is actually running at scale within the organization. This “born from production” attribute is a key reason Goose became one of the first AAIF projects.

This fact alone is worth a closer look.

This article does not aim to prove how powerful Goose is, but rather to answer three more practical questions:

* What overlooked but long-term critical problems does Goose actually solve?
* Why was it Goose, and not another Agent framework, that entered AAIF?
* What does this mean for Agentic Runtime and AI-Native infrastructure, which I care about?

## Goose’s True Positioning: It’s Not an IDE or a Chatbot

If you only look at its surface features, Goose is easily mistaken for one of two things:

* A “multi-model AI desktop client”
* Or an “intelligent programming assistant that can run commands”

But inside Block, it was never designed as a “tool” from the start.

Goose’s origin is closely tied to Block’s engineering environment.

Block (formerly Square) is a classic engineering-driven company: complex systems, high automation needs, many internal tools, and very high execution costs in real production environments. In its recent AI transformation, Block did not focus on “which model to choose” or “which AI tool to introduce,” but directly targeted the engineering execution layer itself.

Goose was born in this context.

Its goal is not to “help people code faster,” but to enable models to **stably and controllably take action**: run tests, modify code, drive UIs, call internal systems, and operate reliably in real engineering environments.

In short:

> Goose is more like an executable Agent Runtime than a conversation-centric product.

## Block’s AI Transformation Started with Organization, Not Tools

To understand Goose, you can’t ignore a key organizational shift at Block.

In an interview with Block’s CTO, one signal was very clear: the starting point for AI transformation was not buying tools or stacking models, but the organizational structure itself.

Block shifted from a business-line GM model to a more functionally oriented structure, making engineering and design the company’s core scheduling units again. This is essentially a proactive response to Conway’s Law.

If the organizational structure doesn’t allow technical capabilities to be orchestrated centrally, Agents will ultimately remain “personal assistants” or “engineering toys.”

From this perspective, Goose is not just a tool, but a **cultural signal**:

> Every employee can use AI to build and execute real system behaviors.

This also explains a fact many overlook:
Goose was not packaged as SaaS, nor was it rushed to commercialization, but was open-sourced and rapidly standardized.

Because its role inside Block is closer to an “operating system for execution models” than a product that can be sold separately.

## Why Did Goose Enter AAIF? Not Because It’s “Technically Strongest”

This is what confuses outsiders the most.

If you only look at flashy features, model support, or community popularity, Goose doesn’t stand out. But AAIF’s choice was not about “maximum capability,” but about **whether the position is right**.

Looking at the first batch of AAIF projects, a clear chain emerges:

* MCP (Anthropic): Defines how models safely and standardly call tools
* AGENTS.md (OpenAI): Defines behavioral conventions for Agents in code repositories
* Goose (Block): A real, runnable, open-source Agent execution framework

Goose’s role is not to set new protocols, but to serve as the **practical carrier and reference implementation** for th...