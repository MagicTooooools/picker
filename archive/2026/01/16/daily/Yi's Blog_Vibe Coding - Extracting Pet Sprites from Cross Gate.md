---
title: Vibe Coding - Extracting Pet Sprites from Cross Gate
url: https://wangyi.ai/blog/2026/01/16/cross-gate-pet-extractor/
source: Yi's Blog
date: 2026-01-16
fetch_date: 2026-01-17T03:30:47.809845
---

# Vibe Coding - Extracting Pet Sprites from Cross Gate

# [Yi's Blog](/)

## ä¸å¾æ°å¥ï¼ä½é®ä¼å£

* [RSS](/atom.xml "subscribe via RSS")

* [Blog](/)
* [Stream](/stream)
* [Archives](/blog/archives)

January
16th
,
2026

# Vibe Coding - Extracting Pet Sprites from Cross Gate

![Cross Gate Pet Viewer](/images/cross-gate-pet-viewer.png)

Cross Gate (é­åå®è´) was one of the most influential MMORPGs in Taiwan and China during the early 2000s. As someone who spent countless hours collecting pets in this game during my childhood, I recently embarked on a nostalgia-driven project: extracting all the pet sprites from the game files and building a modern web viewer to browse them.

## The Challenge

Game resources from the early 2000s are notoriously difficult to work with. Cross Gate uses proprietary binary formats for its graphics and animation data:

* **GraphicInfo\_\*.bin** (40 bytes per entry) - Metadata for each graphic including dimensions, offsets, and addresses
* **Graphic\_\*.bin** - RLE-compressed 8-bit indexed images with transparency
* **AnimeInfo\_\*.bin** (12 bytes per entry) - Animation metadata linking pet IDs to frame sequences
* **Anime\_\*.bin** - Animation frame data with actions and directions
* **Palette files (.cgp)** - 224-color palettes mapping indices 16-239

The compression format is a custom RLE implementation with multiple encoding modes (literal, repeat, transparent) and variable-length counters.

## The Solution

Using AI-assisted development (Claude Code and Antigravity), I built a Python extraction pipeline:

1. **Parse the binary formats** - Read the structured binary files, extracting metadata and addresses
2. **Decompress RLE graphics** - Implement the full RLE decompression algorithm with all encoding modes
3. **Apply palettes** - Map 8-bit indexed pixels to RGB colors using the gameâs palette files
4. **Generate animated GIFs** - Combine frames into animated GIFs for each petâs actions and directions

Each pet has up to 10 actions (Idle, Walk, Attack, Defend, Cast, etc.) and 8 directions, resulting in potentially 80 GIF animations per pet.

## The Frontend

I built a Next.js web application to browse the extracted pets:

* **Grid view** displaying all available pets
* **Detail view** with interactive controls for actions and directions
* **Drag-to-rotate** functionality for intuitive direction changes
* **Pixel-perfect rendering** with `image-rendering: pixelated` to preserve the retro aesthetic

## Lessons Learned

1. **Binary format reverse engineering is time-consuming** - Even with AI assistance, understanding undocumented binary formats requires careful experimentation and validation
2. **Progress persistence is essential** - With 1000+ pets to process, the batch generator needed to skip already-processed pets and handle timeouts gracefully
3. **Test with edge cases early** - Some pets had unusual frame counts or missing animations that caused the initial implementation to fail

## References

This project was made possible by the [cgg-viewer](https://github.com/x2048/cgg-viewer) project, which provided the foundational understanding of Cross Gateâs binary file formats and RLE decompression algorithm. The original Python implementation by the cgg-viewer author was invaluable for understanding how to correctly parse GraphicInfo, AnimeInfo, and palette files.

## Whatâs Next

* [ ] Try [Tencent Hunyuan 3D](https://3d.hunyuan.tencent.com/) to convert 2D sprites into 3D models

You can try it out at <https://1203906e.cross-gate-pets.pages.dev/>.

Posted by Yi

January
16th
,
2026

[English](/category/english/index.html)
 •
[Vibe Coding](/category/vibe-coding/index.html)

[« Breaking Up with Evernote: Building a Custom Migration Tool for Apple Notes](/blog/2026/01/16/evernote-to-apple-notes-migration/ "Previous Post: Breaking Up with Evernote: Building a Custom Migration Tool for Apple Notes")
[Reverse Engineering Guitar Pro 8's Locked Files »](/blog/2026/01/16/unlocking-guitar-pro-8/ "Next Post: Reverse Engineering Guitar Pro 8's Locked Files")

# èç³»æ¹å¼

* wangyi724(at)gmail.com

# Categories

* [éç¬](/category/%C3%A9%C2%9A%C2%8F%C3%A7%C2%AC%C2%94/index.html)
* [å·¥å·æ´»ç¨](/category/%C3%A5%C2%B7%C2%A5%C3%A5%C2%85%C2%B7%C3%A6%C2%B4%C2%BB%C3%A7%C2%94%C2%A8/index.html)
* [Python](/category/python/index.html)
* [iOS](/category/ios/index.html)
* [è¯»ä¹¦ç¬è®°](/category/%C3%A8%C2%AF%C2%BB%C3%A4%C2%B9%C2%A6%C3%A7%C2%AC%C2%94%C3%A8%C2%AE%C2%B0/index.html)
* [è¯æ](/category/%C3%A8%C2%AF%C2%91%C3%A6%C2%96%C2%87/index.html)
* [C++](/category/c/index.html)
* [LeetCode](/category/leetcode/index.html)
* [English](/category/english/index.html)
* [Vibe Coding](/category/vibe-coding/index.html)
* [Vibe](/category/vibe/index.html)
* [Coding](/category/coding/index.html)
* [Reverse Engineering](/category/reverse-engineering/index.html)
* [Guitar Pro](/category/guitar-pro/index.html)

# Recent Posts

* [Reverse Engineering Guitar Pro 8's Locked Files](/blog/2026/01/16/unlocking-guitar-pro-8/)
* [Vibe Coding - Extracting Pet Sprites from Cross Gate](/blog/2026/01/16/cross-gate-pet-extractor/)
* [Breaking Up with Evernote: Building a Custom Migration Tool for Apple Notes](/blog/2026/01/16/evernote-to-apple-notes-migration/)
* [ãä¸ä¸ä¸ºä»ä¹è¦æå¾ä¹¦é¦ãè¯»ä¹¦ç¬è®°](/blog/2025/09/28/library-book-reading-notes/)
* [ãçº³ç¦å°å®å¸ãæ¨èéè¯»](/blog/2025/07/04/na-wa-er-bao-dian-tui-jian-yue-du/)

Copyright © 2026 - Yi -
Powered by [Jekyll](https://jekyllrb.com) and [Octopress](http://octopress.org)