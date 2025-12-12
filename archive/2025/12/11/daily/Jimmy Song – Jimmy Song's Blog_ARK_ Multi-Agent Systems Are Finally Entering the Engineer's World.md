---
title: ARK: Multi-Agent Systems Are Finally Entering the Engineer's World
url: https://jimmysong.io/blog/ark-agent-runtime-engineering/
source: Jimmy Song – Jimmy Song's Blog
date: 2025-12-11
fetch_date: 2025-12-12T03:26:20.129542
---

# ARK: Multi-Agent Systems Are Finally Entering the Engineer's World

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
  中文](/zh/blog/ark-agent-runtime-engineering/)

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
ARK: Multi-Agent Engineering

# ARK: Multi-Agent Systems Are Finally Entering the Engineer's World

How ARK uses cloud-native architecture and declarative runtime to drive engineering adoption of multi-agent systems and shape the Agentic Runtime ecosystem.

Dec 11, 2025
•

[AI Engineering](/categories/ai-engineering)
•

6 Minute
•

1411 words

Table of content

* [Introduction](#introduction)
* [ARK Architecture: Treating Agents as Kubernetes-Native Workloads](#ark-architecture-treating-agents-as-kubernetes-native-workloads)
* [CRD: ARK’s “Abstraction Layer”](#crd-arks-abstraction-layer)
* [Agent Execution Flow: From Query to Tool Invocation](#agent-execution-flow-from-query-to-tool-invocation)
* [The True Value of Multi-Agent: Team Orchestration](#the-true-value-of-multi-agent-team-orchestration)
* [Fundamental Differences Between ARK and Other Frameworks](#fundamental-differences-between-ark-and-other-frameworks)
* [The Engineering Value of ARK](#the-engineering-value-of-ark)
* [Insights for Agentic Runtime](#insights-for-agentic-runtime)
* [Summary](#summary)

> The deep integration of cloud native and AI, with the ARK platform, provides a new paradigm for engineering multi-agent systems.

## Introduction

AI Agents are moving from the “single agent demo” stage to “large-scale operation.” The real challenge does not lie in the model itself, but in engineering issues at runtime: model management, tool invocation, state maintenance, elastic scaling, team collaboration, observability, deployment, and upgrades. These are problems that traditional agent libraries struggle to solve.

**ARK (Agentic Runtime for Kubernetes)** provides a fully operational, observable, governable, and continuously deliverable multi-agent operating system. It is not a Python library, but a complete runtime platform.

![Figure 1: ARK Dashboard](https://assets.jimmysong.io/images/blog/ark-agentic-runtime-for-kubernetes/ark-dashboard-homepage.webp)

Figure 1: ARK Dashboard

Note: In this article, ARK refers to McKinsey’s open-source
[ARK Agent Runtime for Kubernetes](https://github.com/mckinsey/ark-agent-runtime-for-kubernetes).

This article, from an engineer’s perspective, will reorganize ARK’s core capabilities and answer the following questions:

* What engineering challenges does ARK actually solve?
* Why is it worth special attention in the cloud native field?
* How is it fundamentally different from frameworks like LangChain and CrewAI?
* What insights does it offer for the Agentic Runtime ecosystem?

## ARK Architecture: Treating Agents as Kubernetes-Native Workloads

The core idea of ARK is: **An agent is not a script, but a schedulable, governable, and observable Kubernetes workload.**

The following architecture diagram illustrates ARK’s underlying structure.

![Figure 2: ARK Overall Architecture](https://assets.jimmysong.io/images/blog/ark-agentic-runtime-for-kubernetes/168007ae485fa14769e5483aa20805d3.svg)

Figure 2: ARK Overall Architecture

This diagram highlights ARK’s key design points:

* **CRDs declare requirements** (Agent, Model, Team, Tool, Memory, etc.)
* **The Controller translates declarations into actual Pods/Services**
* **The API provides a unified communication entry point and team orchestration**
* **Memory supports long-term state management for agents**
* **MCP Server enables external systems to become tools**
* **Dashboard provides visual management and observability**

ARK adopts the typical cloud-native Operator pattern and applies it to multi-agent systems.

## CRD: ARK’s “Abstraction Layer”

Unlike traditional agent frameworks where “code is logic,” ARK uses CRDs (Custom Resource Definitions) to abstract the components of agent applications.

The main CRD types in ARK include:

* Model
* Agent
* Team
* Tool
* Memory
* Evaluation

These CRDs correspond to all the key components of an agent system.

The following diagram shows the structure of the CRDs:

![Figure 3: CRD Structure (Simplified)](https://assets.jimmysong.io/images/blog/ark-agentic-runtime-for-kubernetes/b464d2b85b6d664b51fa48a5aed2fbd0.svg)

Figure 3: CRD Structure (Simplified)

Through CRDs, ARK achieves the following engineering features:

* **All resources are GitOps-ready**, supporting declarative management
* **Changes are auditable, reversible, and continuously deliverable**
* **The evolution of models, tools, and agents does not require business code changes**

This is the key gene of ARK’s engineering-oriented system.

## Agent Execution Flow: From Query to Tool Invocation

The following image shows how to view query details in the ARK Dashboard.

![Figure 4: Viewing Query Details in ARK Dashboard](https://assets.jimmysong.io/images/blog/ark-agentic-runtime-for-kubernetes/ark-dashboard-queries.webp)

Figure 4: Viewing Query Details in ARK Dashboard

In ARK, the complete execution flow for an agent receiving a query is as follows:

![Figure 5: Agent Execution Flow](https://assets.jimmysong.io/images/blog/ark-agentic-runtime-for-kubernetes/67a8b1142ee63f7cacd4d907cd198ce4.svg)

Figure 5: Agent Execution Flow

This flow has the following characteristics:

* **Memory modules are naturally involved in the execution flow, without code specialization**
* **Large language model (LLM, Large Language Model) and tool invocation are governed by the runtime**
* **Agents can reside in Pods long-term, not just as one-off processes**

This makes ARK more like an “agent microservice platform.”

Below is an example of a request and response:

▸
Request and Response Example

```
kubectl describe query test-password-reset
Name:         test-password-reset
Namespace:    default
Labels:       <none>
Annotations:  <none>
API Version:  ark.mckinsey.com/v1alpha1
Kind:         Query
Metadata:
  Creation Timestamp:  2025-12-11T11:16:45Z
  Finalizers:
    ark.mckinsey.com/finalizer
  Generation:        2
  Resource Version:  63109
  UID:               52bf94fc-cda2-48a7-9d2f-085489fc4877
Spec:
  Input:  How do I reset my password?
  Targets:
    Name:   ...