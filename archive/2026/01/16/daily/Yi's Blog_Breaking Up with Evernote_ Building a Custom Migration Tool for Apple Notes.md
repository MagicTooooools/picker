---
title: Breaking Up with Evernote: Building a Custom Migration Tool for Apple Notes
url: https://wangyi.ai/blog/2026/01/16/evernote-to-apple-notes-migration/
source: Yi's Blog
date: 2026-01-16
fetch_date: 2026-01-17T03:30:48.220960
---

# Breaking Up with Evernote: Building a Custom Migration Tool for Apple Notes

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

# Breaking Up with Evernote: Building a Custom Migration Tool for Apple Notes

After 15+ years of note-taking, I finally said goodbye to Evernote. Hereâs the technical journey of migrating 4,330 notesâwith all their attachments, tables, and formattingâto Apple Notes.

## The Problem

Evernote had been my digital brain since the late 2000s. But with each passing version, the app became slower, more bloated, and increasingly expensive. Apple Notes, meanwhile, has quietly evolved into a capable, fast, and free alternative that syncs seamlessly across my devices.

The catch? **Thereâs no official migration path.** Evernoteâs export format (ENEX) doesnât preserve everything, and Apple Notes doesnât have any bulk import feature. Manual copy-paste wasnât an option.

So I built my own migration tool.

## What Made This Hard

This wasnât a simple file conversion:

* **Rich text formatting** including tables, checklists, and styled text
* **Embedded attachments** (images, PDFs, documents) referenced by MD5 hashes in Evernoteâs proprietary ENML format
* **Creation and modification dates** that needed to be preserved
* **Duplicate detection** to allow resumable, interruptible migrations
* **Apple Notesâ limitations**âno public API, only AppleScript access

Evernote v10 made things even more complicated. Unlike older versions that stored everything in a straightforward SQLite database, v10 uses a hybrid system with:

* A SQLite database for metadata
* Separate `.dat` files containing rich text content (tables/formatting)
* Protobuf-encoded binary structures
* Server-side attachment storage requiring authenticated downloads

## The Solution: A Two-Phase Migration System

I built a Python-based migration pipeline that handles all of this complexity.

### Phase 1: Parallel Preparation

The first phase downloads attachments and generates PDFs in parallel using 10 worker threads. For notes with embedded images or files, I render the complete content (HTML + attachments) into a PDF using headless Chrome. This preserves formatting perfectly.

### Phase 2: Sequential Import

The second phase imports to Apple Notes via AppleScriptâsequentially, because Apple Notes doesnât handle concurrent modifications well.

### Solving the Attachment Problem

Evernote embeds attachments using `<en-media>` tags with MD5 hashes. To resolve these to actual files, I:

1. Query Evernoteâs local database for attachment metadata
2. Download from Evernoteâs servers using captured auth tokens
3. Embed them as base64 in generated PDFs
4. Attach the PDF to the Apple Notes entry

### Deduplication Done Right

My initial attempt at duplicate detection was fragileâcomparing dates via AppleScript often failed. The fix was simple: track Evernote note IDs in a log file. This makes the migration **fully resumable**.

## Bonus: AI-Powered Organization

Once notes were in Apple Notes, I used Gemini AI to automatically categorize them into folders based on content.

## Lessons Learned

1. **AppleScript is slow but reliable** â Building a cache at startup dropped duplicate checks from 0.5s to 0.001s per note.
2. **Parallelism for I/O, sequential for mutations** â Downloading attachments scales linearly with workers. Writing to Apple Notes must be sequential.
3. **Auth tokens expire** â Evernoteâs tokens last about an hour. I kept Proxyman ready to capture fresh tokens.
4. **PDF is a universal container** â When your target doesnât support rich formatting or attachments, bundle everything into a PDF.

## The Code

The entire migration toolkit is available on GitHub: [apple-notes-toolkit](https://github.com/Jeswang/apple-notes-toolkit)

â ï¸ Note: This repo is fully vibe coded. Use with caution.

## Final Thoughts

What started as a weekend project turned into a deep dive into Evernoteâs internals, Appleâs Scripting Bridge, and the art of data migration. But the result is worth it: my 15 years of notes are now in Apple Notes, fully searchable, syncing across devices, andâmost importantlyâmine to keep.

If youâre considering leaving Evernote, know that itâs possible. It just takes a bit of engineering.

Posted by Yi

January
16th
,
2026

[English](/category/english/index.html)
 •
[Vibe Coding](/category/vibe-coding/index.html)

[« ãä¸ä¸ä¸ºä»ä¹è¦æå¾ä¹¦é¦ãè¯»ä¹¦ç¬è®°](/blog/2025/09/28/library-book-reading-notes/ "Previous Post: ãä¸ä¸ä¸ºä»ä¹è¦æå¾ä¹¦é¦ãè¯»ä¹¦ç¬è®°")
[Vibe Coding - Extracting Pet Sprites from Cross Gate »](/blog/2026/01/16/cross-gate-pet-extractor/ "Next Post: Vibe Coding - Extracting Pet Sprites from Cross Gate")

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