---
title: Can Open Source Suddenly Disappear? An AI Chat Dev Tool Went 404 Overnight
url: https://jimmysong.io/blog/ai-project-lunary-404/
source: Jimmy Song – Jimmy Song's Blog
date: 2025-12-11
fetch_date: 2025-12-12T03:26:20.528047
---

# Can Open Source Suddenly Disappear? An AI Chat Dev Tool Went 404 Overnight

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
  中文](/zh/blog/ai-project-lunary-404/)

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
Can Open Source Suddenly Disappear? An AI Chat Dev Tool Went 404 Overnight

# Can Open Source Suddenly Disappear? An AI Chat Dev Tool Went 404 Overnight

Lunary, an open-source project in the AI DevTool space, suddenly deleted its GitHub repo, exposing the instability of commercial open source projects.

Dec 11, 2025
•

[AI Engineering](/categories/ai-engineering)
•

4 Minute
•

862 words

Table of content

* [The Disappearance of Lunary’s Repository: A Real Case of Open Source “Vanishing”](#the-disappearance-of-lunarys-repository-a-real-case-of-open-source-vanishing)
* [Lunary’s Positioning and Features](#lunarys-positioning-and-features)
* [The Reality and Risks Behind the “Open Source” Label](#the-reality-and-risks-behind-the-open-source-label)
* [Possible Industry Reasons for Repo Deletion](#possible-industry-reasons-for-repo-deletion)
* [The “Pseudo Open Source” Phenomenon in AI Tools](#the-pseudo-open-source-phenomenon-in-ai-tools)
* [Three Practical Lessons for Developers](#three-practical-lessons-for-developers)
* [My First Experience Maintaining an AI Project List and Facing Repo Deletion](#my-first-experience-maintaining-an-ai-project-list-and-facing-repo-deletion)
* [Summary](#summary)

> “Open source” in the AI era is no longer a trustworthy promise. Commercial projects can withdraw their code at any time, and developers must be wary of the gap between appearances and reality.

## The Disappearance of Lunary’s Repository: A Real Case of Open Source “Vanishing”

While updating the AI open source project library on my website, I encountered a situation that left me stunned for the first time:
An “open-source AI tool” that still promotes itself, with an active website and commercial services, suddenly vanished from GitHub—its repository went straight to 404.

The project is called Lunary.

Original repository address:
<https://github.com/lunary-ai/lunary>It now returns a 404 Not Found.

Notably, the official site lunary.ai remains online, but the core promise of an “open-source codebase” has disappeared.

## Lunary’s Positioning and Features

Here is an overview of Lunary’s main features and positioning to help understand its role in the AI tool ecosystem.

Lunary claims to be an Observability and Evaluations platform for large language model (LLM, Large Language Model) applications, focusing on:

* LLM conversation and feedback logs
* Cost, latency, and metrics analysis
* Prompt version management
* Distributed tracing
* Evaluations
* Supports both self-hosted and managed modes
* Provides JS / Python SDKs, integrates with LangChain

Its overall positioning is clear:
“Development and debugging tools for AI applications.”

In fact, products like this have emerged rapidly over the past year, forming a new AI DevTool track.

## The Reality and Risks Behind the “Open Source” Label

The core issue is not the tool itself, but its claim to be “open source.”

Lunary has consistently emphasized:

> “Lunary is an open-source platform for developers.”

This statement is great for attracting users, as open source implies transparency, trustworthiness, self-hosting, and community participation.

But now the repository is gone, with only the website continuing its promotion—raising many questions.

Lunary is not a niche hobby project, but a commercial company-led initiative. If an individual suddenly deletes a repo, it’s not surprising, but for a company operating publicly, this move is extremely rare.

This is the first time I’ve truly seen a reality in the AI DevTools space: “Open source” is being used as a branding term, not a commitment.

## Possible Industry Reasons for Repo Deletion

Let’s analyze some common industry reasons for deleting a repository to help developers understand the motivations behind such actions.

* **Increased commercial pressure**: These tools often struggle with sustainable business models, prompting teams to shift to closed-source SaaS.
* **Pivoting**: The company finds the original direction unprofitable and prepares to change course.
* **Team changes**: Acquisition, key member departures, or funding issues can all lead to repo shutdowns.
* **Compliance or legal risks**: Observability products involve user data, which may require public code to be taken down.

Regardless of the reason, the impact on users is the same: it is no longer an “open-source product.”

## The “Pseudo Open Source” Phenomenon in AI Tools

The most noteworthy aspect is not Lunary itself, but the rapid spread of this phenomenon in the AI tool space.

Many projects use “open source” as a user acquisition strategy but lack open governance and long-term commitment.

High substitutability, homogeneity, and commercial pressure mean these DevTools have low survival rates.

When commercial teams lead open source, a single decision can make the repository disappear instantly.

In the cloud native era, we’ve already seen a wave of “pseudo open source.” In the AI era, this trend is accelerating.

## Three Practical Lessons for Developers

Based on this case, here are three practical lessons for developers:

* **The “open source label” does not guarantee trustworthiness**: Open source projects led by commercial companies without community or foundation backing can be withdrawn at any time.
* **AI DevTools are far less stable than infrastructure**: These tools are not essential, highly replaceable, and have short lifecycles.
* **Tool usability should take precedence over “open source status”**: Because it may stop being open source at any moment.

## My First Experience Maintaining an AI Project List and Facing Repo Deletion

After collecting hundreds of projects over the past two years, this is the first time I’ve encountered a “commercial open source project disappearing, official repo 404” case.

To me, this is an industry signal: the AI open source world is entering a period of drift, and commercial projects’ open source commitments are increasingly unstable.

It also reminds everyone making technical choices: in the AI era...