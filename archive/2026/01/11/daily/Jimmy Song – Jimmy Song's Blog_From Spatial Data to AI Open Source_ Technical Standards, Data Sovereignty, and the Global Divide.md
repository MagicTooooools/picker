---
title: From Spatial Data to AI Open Source: Technical Standards, Data Sovereignty, and the Global Divide
url: https://jimmysong.io/blog/spatial-data-ai-open-source-standards-sovereignty/
source: Jimmy Song – Jimmy Song's Blog
date: 2026-01-11
fetch_date: 2026-01-12T03:39:52.585268
---

# From Spatial Data to AI Open Source: Technical Standards, Data Sovereignty, and the Global Divide

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
  + [Glossary

    Bilingual terminology glossary for translation and writing reference.](/glossary/)
* [About](/about/)
* [中文
  中文](/zh/blog/spatial-data-ai-open-source-standards-sovereignty/)

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
From Spatial Data to AI Open Source: Technical Standards, Data Sovereignty, and the Global Divide

# From Spatial Data to AI Open Source: Technical Standards, Data Sovereignty, and the Global Divide

How technical standards and data sovereignty shape AI open source paths and infrastructure competition in the global AI era.

Jan 11, 2026
•

[Open Source](/categories/open-source)
•

9 Minute
•

1932 words

Table of content

* [Author’s Note](#authors-note)
* [Differences in Air Quality Data Presentation: A Microcosm of Technical Standards and Sovereignty](#differences-in-air-quality-data-presentation-a-microcosm-of-technical-standards-and-sovereignty)
* [Air Quality Is Just a Slice: Greater Differences in Spatial Public Data](#air-quality-is-just-a-slice-greater-differences-in-spatial-public-data)
* [Three Global Paths](#three-global-paths)
* [The Essence of “Point” vs. “Area” in Air Quality](#the-essence-of-point-vs-area-in-air-quality)
* [The Amplification Effect in the AI Era](#the-amplification-effect-in-the-ai-era)
* [The US Approach: Focusing on Rules and Runtime Layers](#the-us-approach-focusing-on-rules-and-runtime-layers)
* [China’s Shift: From Model-Oriented to Infrastructure-Oriented](#chinas-shift-from-model-oriented-to-infrastructure-oriented)
* [Exploration and Practice at the Infrastructure Layer](#exploration-and-practice-at-the-infrastructure-layer)
* [Realistic Assessment: The Starting Point of AI Infrastructure Competition](#realistic-assessment-the-starting-point-of-ai-infrastructure-competition)
* [Summary](#summary)

> The divide in technical standards and data sovereignty determines the global competitive landscape of infrastructure open source in the AI era.

In this article, I will use the differences in air quality data presentation in Apple Maps and Weather as a starting point to explore how technical standards and data sovereignty influence the open source paths of AI in different countries. I will further analyze why, in the AI era, infrastructure-level open source has become the key battleground for ecosystem dominance.

## Author’s Note

This article originates from a very everyday observation: Why is air quality data in China shown as “points” in Apple Maps and Weather, while in other countries it is often displayed as “areas”?

![Figure 1: Air quality map in Apple Weather, showing point-based data in China and area-based data in other countries](https://assets.jimmysong.io/images/blog/spatial-data-ai-open-source-standards-sovereignty/aqi-map.webp)

Figure 1: Air quality map in Apple Weather, showing point-based data in China and area-based data in other countries

At first glance, it seems like a product experience difference. But when I reconsidered this issue in the context of engineering, standards, and system design, I realized it actually points to a much bigger question: how different countries understand the relationship between technology, standards, openness, and sovereignty.

As an engineer who has long worked in cloud native, AI infrastructure, and open source ecosystems, I gradually realized that this difference is not limited to air quality or map data. In the AI era, it is further amplified, directly affecting how we open source models, build infrastructure, and whether we can participate in the formulation of global rules.

Writing this article is not about judging right or wrong, but about using a concrete example to explain a structural difference and discuss the long-term impact and real opportunities this difference may bring in the AI era.

What is especially important: at the level of AI infrastructure and infra-level open source, the competition has just begun. China is not without opportunities, but the choice of path will become more critical than ever.

## Differences in Air Quality Data Presentation: A Microcosm of Technical Standards and Sovereignty

The following image illustrates the divide between spatial data, AI open source, and technical standards. By comparing how air quality data is presented in Apple Maps and Weather in different countries, you can intuitively feel the differences in technical standards and sovereignty strategies behind the scenes.

![Figure 2: The divide between spatial data, AI open source, and technical standards](https://assets.jimmysong.io/images/blog/spatial-data-ai-open-source-standards-sovereignty/banner.webp)

Figure 2: The divide between spatial data, AI open source, and technical standards

If you regularly use global products such as maps, weather, traffic, or various data services, you may notice a recurring phenomenon that is rarely discussed seriously: the way data is presented in China often differs significantly from global mainstream standards.

A very intuitive example comes from the air quality display in Apple Maps or Weather. In China, air quality is usually shown as discrete points; in the US, Europe, Japan, and other countries, it is often rendered as continuous coverage areas.

At first glance, this seems like a product experience difference, and may even lead people to mistakenly believe that “China’s data is incomplete.” But if you treat it as an engineering or system design issue, you will find: this is not a matter of data capability, but a different choice in technical standards, data sovereignty, and openness strategies.

And this choice is not limited to air quality.

## Air Quality Is Just a Slice: Greater Differences in Spatial Public Data

Air quality is just a highly visible and...