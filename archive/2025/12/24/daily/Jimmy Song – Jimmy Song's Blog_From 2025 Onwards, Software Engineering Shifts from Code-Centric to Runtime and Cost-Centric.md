---
title: From 2025 Onwards, Software Engineering Shifts from Code-Centric to Runtime and Cost-Centric
url: https://jimmysong.io/blog/software-engineering-shift-runtime-cost-2025/
source: Jimmy Song – Jimmy Song's Blog
date: 2025-12-24
fetch_date: 2025-12-25T03:30:06.866895
---

# From 2025 Onwards, Software Engineering Shifts from Code-Centric to Runtime and Cost-Centric

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
  中文](/zh/blog/software-engineering-shift-runtime-cost-2025/)

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
2025 Year in Review: How AI Is Shifting the Focus of Software Engineering

# From 2025 Onwards, Software Engineering Shifts from Code-Centric to Runtime and Cost-Centric

In 2025, software engineering shifts from code-centric to runtime and cost governance. AI and Agents move complexity to runtime, compute, and budget layers, reshaping engineering value.

Dec 24, 2025
•

[AI Engineering](/categories/ai-engineering)
•

4 Minute
•

816 words

Table of content

* [My 2025: From “Platform Engineering” to “Runtime Challenges”](#my-2025-from-platform-engineering-to-runtime-challenges)
* [Industry Consensus: AI Is Shifting the Focus of Engineering](#industry-consensus-ai-is-shifting-the-focus-of-engineering)
* [Why “Cost” Becomes a First Principle](#why-cost-becomes-a-first-principle)
* [The Rise of Agents: The Real Challenge Is at Runtime](#the-rise-of-agents-the-real-challenge-is-at-runtime)
* [Outlook for 2026: The “Foundation” of Engineering Matters Again](#outlook-for-2026-the-foundation-of-engineering-matters-again)
* [Summary](#summary)

> In 2025, the core of software engineering is no longer just about code itself, but about runtime controllability and cost governance. This shift is fundamentally reshaping the industry’s underlying logic.

Looking back at 2025, I became increasingly aware that this year was not about “code becoming unimportant,” but rather that **the value coordinates of engineering have shifted as a whole**. For more than a decade, software engineering has focused on code quality, architectural evolution, and delivery efficiency. But starting in 2025, the key to system success is shifting—**towards whether the runtime is controllable and whether costs are governable**.

This is not just a slogan, but a conclusion repeatedly validated by my real-world experiences throughout the year.

## My 2025: From “Platform Engineering” to “Runtime Challenges”

In my annual review, I noted a clear change: I spent less time on “how to write a good system,” and more time on “how to keep the system running stably, reliably, and affordably.”

This shift in focus is a natural extension of a decade of cloud native evolution.

The following timeline diagram illustrates how my focus has changed over recent years:

![Figure 1: My Focus Shift Timeline](https://assets.jimmysong.io/images/blog/software-engineering-shift-runtime-cost-2025/ec83228ec9977169ccab2e4bda327c7e.svg)

Figure 1: My Focus Shift Timeline

When AI workloads truly enter business scenarios, the core challenges engineers face also change:

* Are inference, training, and evaluation competing for the same compute pool?
* Is GPU utilization consistently below expectations?
* Does cost scale linearly and uncontrollably with concurrency?
* Does the system have failure isolation and replay capabilities?

These issues go far beyond the code level.

## Industry Consensus: AI Is Shifting the Focus of Engineering

By 2025, an industry consensus is emerging: AI is rewriting software engineering. But the real change is not happening in the IDE or code completion speed—it is reflected in **the migration of engineering complexity**.

Previously, complexity was concentrated in code and interfaces, and problems were solved through abstraction, refactoring, and testing.

Now, complexity has shifted to the runtime, resource, and cost layers, and must be addressed through scheduling, isolation, observability, and governance.

This is why the same AI tools:

* Serve as “accelerators” for junior engineers
* But act as “magnifiers” for senior engineers

AI tools amplify whether you truly understand how systems run in production.

## Why “Cost” Becomes a First Principle

In traditional cloud native systems, low CPU utilization is often just an efficiency issue; but in AI systems, **low GPU utilization is often a cash flow problem**.

In 2025, I repeatedly encountered scenarios like:

* Resources “seem insufficient,” but utilization is not actually high
* Scaling up to solve queuing issues ends up increasing unit costs
* The system lacks clear budget and quota boundaries, so throttling becomes the only way to stop the bleeding

The root cause of these phenomena is not model selection, but **the lack of a runtime and cost control plane tailored for AI workloads**.

The following flowchart visually illustrates the cyclical relationship between GPU resources and cost pressures:

![Figure 2: GPU Resource and Cost Cycle in AI Systems](https://assets.jimmysong.io/images/blog/software-engineering-shift-runtime-cost-2025/192c24b80f6365870ee84639f648b3e5.svg)

Figure 2: GPU Resource and Cost Cycle in AI Systems

Engineering problems ultimately manifest as cost issues.

## The Rise of Agents: The Real Challenge Is at Runtime

In 2025, Agent (Intelligent Agent, Agent, Intelligent Agent) became a hot topic; by 2026, it will enter the “can it actually run” stage.

The challenge for Agents has never been about “how smart they are,” but rather:

* Whether there are clear permission and data boundaries
* Whether they run in an isolated execution environment
* Whether they can be observed, evaluated, and replayed
* Whether they are subject to explicit cost and budget constraints

These capabilities form the outline of **Agentic Runtime (Agentic Runtime, Intelligent Agent Runtime)** that I have been trying to clarify throughout the year.

The following flowchart shows the core capability layers of Agentic Runtime:

![Figure 3: Agentic Runtime Capability Layers](https://assets.jimmysong.io/images/blog/software-engineering-shift-runtime-cost-2025/aa2e88b6cc252e6d3a3b7934ae6c1331.svg)

Figure 3: Agentic Runtime Capability Layers

Without a runtime, an Agent is just a demo; without cost constraints, an Agent is just a risk amplifier.

## Outlook for 2026: The “Foundation” of Engineering Matters Again

Looking ahead to 2026, I remain cautiously optimistic.

I do not believe the future belongs to “those...