---
title: From Cloud Native to AI Native: Why Kubernetes Is the Foundation for Next-Gen AI Agents
url: https://jimmysong.io/blog/ai-native-from-cloud-native/
source: Jimmy Song – Jimmy Song's Blog
date: 2025-12-24
fetch_date: 2025-12-25T03:30:07.093388
---

# From Cloud Native to AI Native: Why Kubernetes Is the Foundation for Next-Gen AI Agents

[Read: From using AI to building AI systems](/blog/from-using-ai-to-building-ai-systems/), a defining note on what I’m exploring.

[![Brand Icon](/images/icons/jimmysong.svg)
AI NATIVE INFRA](/)

Search
⌘K

* [Blog](/blog/)

  ### Explore the Blog

  Dive into deep analyses on AI, cloud native and industry trends.

  [View all](/blog/)

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

  + [AI OSS Landscape

    AI resources for developers and tech enthusiasts.](/ai/)
  + [ArkSphere AI

    AI Native Infra, Agentic Runtime community.](/community/)
  + [Travel Stories

    Discover experiences and journeys beyond technology.](/travel/)
* [About](/about/)
* [中文
  中文](/zh/blog/ai-native-from-cloud-native/)

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
From Cloud Native to AI Native: Why Kubernetes Is the Foundation for Next-Gen AI Agents

# From Cloud Native to AI Native: Why Kubernetes Is the Foundation for Next-Gen AI Agents

Explores why AI Agents need Kubernetes infrastructure and how Agent orchestration, MCP services, and AI gateways enable production-ready AI architectures.

Dec 24, 2025
•

[AI Engineering](/categories/ai-engineering)
•

3 Minute
•

748 words

Table of content

* [Cloud Native Challenges for Production-Grade AI Agents](#cloud-native-challenges-for-production-grade-ai-agents)
* [Core Architecture: Making Agents Native Kubernetes Objects](#core-architecture-making-agents-native-kubernetes-objects)
* [Agent Orchestration Layer: Declarative Agent Management](#agent-orchestration-layer-declarative-agent-management)
  + [Agents as Kubernetes Resources](#agents-as-kubernetes-resources)
* [Tool Service-ization Layer: MCP Services Are Essential](#tool-service-ization-layer-mcp-services-are-essential)
  + [The Essence of MCP Service-ization](#the-essence-of-mcp-service-ization)
* [AI Native Gateway: The “Control Plane Entry” for the Agent World](#ai-native-gateway-the-control-plane-entry-for-the-agent-world)
* [Architecture Overview](#architecture-overview)
* [Summary](#summary)

> As a long-time practitioner in the cloud native field, I am increasingly convinced of one thing: **AI Agents are not just a change in application form, but a migration of infrastructure paradigms.**

As artificial intelligence evolves from demos and copilots to systems that truly take on tasks and responsibilities, **AI Agents** are becoming the new execution units in enterprise IT architectures. They not only “think,” but also **act**: they can invoke tools, access systems, and collaborate to achieve goals.

This raises an important question:

> **What kind of infrastructure should such systems run on?**

In my view, Kubernetes remains a solid choice for large-scale scenarios—but only if we **reimagine Kubernetes in an AI-native way**.

## Cloud Native Challenges for Production-Grade AI Agents

In real production environments, AI Agents expose infrastructure needs that are fundamentally different from traditional microservices. Agents are not “just another HTTP service”; they have three distinct characteristics:

* **Behavior is non-deterministic** (driven by model inference)
* **Execution paths are dynamic** (tool invocation cannot be fully enumerated in advance)
* **Decisions must be auditable, constrained, and reviewable**

If we simply apply existing cloud native infrastructure, we quickly hit bottlenecks.

The following table summarizes the main challenges and risks AI Agents face in cloud native environments:

| Challenge Category | Real Needs of Agents | What Happens If Missing |
| --- | --- | --- |
| **Policy & Security** | Dynamic control of tool and data access based on context, identity, and task | Agents have “superuser” privileges, risks are uncontrollable |
| **Observability** | Not just “did it succeed,” but also **why was this decision made** | Hard to debug, hard to review, hard to hold accountable |
| **Governance & Consistency** | Platform-level guardrails enforce organizational policies | Each Agent could become a “shadow AI” |

Table 1: Challenges and Risks for AI Agents in Cloud Native Environments

All these issues point to one conclusion:

> **AI Agents must be treated as first-class citizens in Kubernetes, not just ordinary workloads.**

## Core Architecture: Making Agents Native Kubernetes Objects

Looking back at the evolution of cloud native technologies, we’ve gone through similar stages:

* Physical machines → Virtual machines
* Virtual machines → Containers
* Containers → Microservices
* Microservices → Declarative, governable platforms

**AI Agents are simply the next step.**

A production-ready AI Agent architecture requires at least three layers:

1. **Agent Orchestration Layer**: Declaratively define Agents
2. **Tool Service-ization Layer (MCP Services)**: Turn capabilities into governable services
3. **AI Native Data Plane / Gateway**: Unify policy, security, and protocols

## Agent Orchestration Layer: Declarative Agent Management

Agents should no longer be “runtime objects” inside an SDK—they should be managed like Pods or Deployments.

Key concepts:

### Agents as Kubernetes Resources

* Agents are defined using **CRD (CustomResourceDefinition)**
* Lifecycle managed via `kubectl` or GitOps
* Agent **models, tools, and policies** are all explicitly declared

A typical Agent definition includes:

* **Agent logic** (inference loop)
* **Model configuration** (specifying which large language model to use)
* **Callable toolset**

> This closely mirrors how we once decomposed “applications” into Deployments, Services, and ConfigMaps.

## Tool Service-ization Layer: MCP Services Are Essential

In Agent architectures, **tools** are where real “actions” happen.

Early MCP tools were often:

* Local processes
* Tightly coupled to a single Agent
* Lacking versioning, permissions, and auditing

This is unsustainable in enterprise environments.

### The Essence of MCP Service-ization

* Tools → **Remote services**
* Services → **Kubernetes native workloads**
* Capabilities → **Reusable, governable, auditable**

This step is fundamentally similar to how we once turned scripts into microservices.

## AI Native Gateway: The “Control Plane Entry” for the Agent World

As the number of Agents grows and tools/models diversify, **connectivity itself becomes a system risk**.

Traditional API Gateways do not understand scenarios like:

* MCP
* Agent-to-Agent (A2A) communication
* Model invocation context

Thus, we need an **AI native gateway** dedicated to mediation and governance.

It must understand at least three types of traffic:

* **A2T**: Agent → Tool
* **A2L**: Agent → LLM
* **A2A**: Agent ↔ Agent

And enforce, across these paths:

* Identity and authorization
* Poli...