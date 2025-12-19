---
title: What I Saw at COSCon'25: The Real State of Open Source in China
url: https://jimmysong.io/blog/coscon-2025-china-open-source-observation/
source: Jimmy Song – Jimmy Song's Blog
date: 2025-12-18
fetch_date: 2025-12-19T03:25:29.489135
---

# What I Saw at COSCon'25: The Real State of Open Source in China

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

  + [AI Resources

    AI resources for developers and tech enthusiasts.](/ai/)
  + [ArkSphere AI

    AI Native Infra, Agentic Runtime community.](/community/)
  + [Travel Stories

    Discover experiences and journeys beyond technology.](/travel/)
* [About](/about/)
* [中文
  中文](/zh/blog/coscon-2025-china-open-source-observation/)

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

[Home](/)[Blog](/blog/)[Open Source](/categories/open-source/)
What I Saw at COSCon'25: The Real State of Open Source in China

# What I Saw at COSCon'25: The Real State of Open Source in China

From an engineering and organizer’s perspective, real changes at COSCon'25: AI as the default backdrop, discussions returning to engineering issues, and Chinese open source entering a long-term phase.

Dec 18, 2025
•

[Open Source](/categories/open-source)
•

4 Minute
•

912 words

Table of content

* [This COSCon: No More Trying to “Prove Open Source Matters”](#this-coscon-no-more-trying-to-prove-open-source-matters)
* [AI as Background Noise, Not the Main Character](#ai-as-background-noise-not-the-main-character)
* [Real Impressions as a Cloud Native Sub-forum Producer](#real-impressions-as-a-cloud-native-sub-forum-producer)
  + [First, Topics Clearly Converged on “Engineering Problems”](#first-topics-clearly-converged-on-engineering-problems)
  + [Second, The Boundary Between Academia and Industry Is Thinning](#second-the-boundary-between-academia-and-industry-is-thinning)
  + [Third, Open Source Is No Longer Just About Code](#third-open-source-is-no-longer-just-about-code)
* [Main Forum: More Questions, Not Answers](#main-forum-more-questions-not-answers)
* [Exhibition Area and Sub-forums: Closer to the Real Ecosystem](#exhibition-area-and-sub-forums-closer-to-the-real-ecosystem)
* [A Personal Judgment](#a-personal-judgment)

> Attending COSCon'25 in Beijing, I observed firsthand how open source in China is shifting: AI is now the default context, discussions are grounded in real engineering, and the community is embracing long-term thinking. These are not just trends—they are the new reality.

In early December this year, I attended COSCon'25, the China Open Source Annual Conference, in Beijing. Although I have worked in open source for many years, this was my first time participating in an event organized by the Open Source Society—and I joined as a sub-forum producer. Previously, I thought such conferences were too high-level or disconnected from reality, but after actually taking part, I found there was much to gain.

A quick note: **this article is not an official conference summary or review**. The organizers have already published detailed information about the event’s scale, attendee numbers, and forum sessions. If you’re interested in those details, please refer to the official article:
[COSCon'25: The 10th China Open Source Annual Conference Successfully Concludes in Beijing—A Comprehensive Recap!](https://mp.weixin.qq.com/s/1Q5xBUEmSN9MXon03P00lA)

![Figure 1: 10th COSCon Venue](https://assets.jimmysong.io/images/blog/coscon-2025-china-open-source-observation/banner.webp)

Figure 1: 10th COSCon Venue

What I want to share is this: **Standing on site, on the engineering front lines, and as an organizer rather than an audience member, I saw real changes happening in Chinese open source.**

## This COSCon: No More Trying to “Prove Open Source Matters”

One clear impression:
**Almost no one spent time arguing “why do open source” anymore.**

In earlier years, common narratives included:

* Is open source safe?
* Can open source be commercialized?
* Can China create its own open source projects?

But at COSCon'25, these questions were basically assumed as “background conditions.” The focus shifted to **those already doing open source, and what comes next**.

This doesn’t mean the issues have disappeared, but it does mean:

> In China’s engineering circles, open source is no longer a “philosophical choice”—it’s a practical way of working.

## AI as Background Noise, Not the Main Character

The theme of this year’s conference was Open Source × Open Intelligence, but interestingly, **AI did not take center stage**.

Instead, it was more like background noise—
Almost every topic touched on AI, but no one was giving talks solely “about AI.”

You would see it repeatedly in areas like:

* Cloud native scheduling, focusing on GPU / NPU / heterogeneous resources
* Storage and data, focusing on data paths for training and inference
* Serverless, focusing on LLM cold starts and elasticity
* Observability, focusing on what to do when system complexity gets out of hand

AI was not treated as a “hot trend,” but as **a new workload reality**.
This is a significant change, though not one easily captured in press releases.

## Real Impressions as a Cloud Native Sub-forum Producer

I helped organize the cloud native open source sub-forum at this year’s conference. This role gave me a perspective very different from that of a typical attendee.

### First, Topics Clearly Converged on “Engineering Problems”

There were almost no talks about Kubernetes concepts;
Very few about “architectural philosophies.”

Instead, the focus was on:

* What pitfalls did you encounter at what scale?
* Why did you choose this solution over another?
* Which problems remain unsolved?

Many presentations weren’t “pleasant to hear,” but they were very real.

### Second, The Boundary Between Academia and Industry Is Thinning

This was especially evident this year.

Some talks from universities and research institutes were no longer just “from a paper’s perspective,” but directly addressed core issues in industrial systems, such as:

* Cold start of serverless LLMs
* The real value of RDMA in inference paths
* Whether prefill/decode separation is truly feasible in engineering

These topics may not be immediately applicable, but **they are now colliding head-on with engineering problems**, rather than talking past each other.

### Third, Open Source Is No Longer Just About Code

In many discussions, “governance,” “maintenance cost,” and “community collaboration” came up frequently.

This is a signal:
When a project is truly being used, **code is no longer the hardest part**.

## Main Forum: More Questions, Not Answers

If I had to sum up the main forum in one sentence:
**It kept raising questions, but wasn’t in a hurry to provide answers...