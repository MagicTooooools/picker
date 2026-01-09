---
title: The Cursor Moment in Music Production
url: https://jw1.dev/cursor-moment-in-music-production
source: Jacky Wong
date: 2026-01-08
fetch_date: 2026-01-09T03:35:10.631136
---

# The Cursor Moment in Music Production

[Jump to the main content](#app)

Full-stack Developer

/

Music Producer

[Home](/)

[Posts](/posts)

[Inspirations](/inspirations)

[RSS](/atom.xml)

# The Cursor Moment in Music Production

January 8, 2026

Looks like your browser has javascript disabled, it's ok, you can still browse this blog.

I’ve been thinking about this for a while.

Cursor didn’t change programming because it could write code. It changed programming because it **made real work faster while keeping every line editable**. That’s the key.

So when does music production get its Cursor moment?

---

## My imagination of this AI DAW

Not magic. **Delegation with control.** And the difficulty scales fast.

**Level 1:** “Generate a 4-bar piano MIDI with emotion.”

Already harder than it sounds. Pitch, velocity, note length, micro-timing, articulation, envelopes. Emotion isn’t metadata, it’s embedded in low-level decisions. Like writing a small utility function. Simple scope, high quality bar.

**Level 2:** “Generate a 16-bar violin MIDI that matches the drum.”

Everything from Level 1, plus context. Groove awareness, phrasing, rhythmic interaction. The model has to listen. Like adding a feature that integrates with an existing module.

**Level 3:** “Generate a sequence using Serum, make bars 8-16 flow, sidechain from track 2, match the vibe.”

This is the inflection point.

Now the AI needs full DAW access, third-party plugin knowledge, routing logic, arrangement continuity, aesthetic coherence. Plugins become libraries. You need something like documentation context for tools, not just parameters. Multi-system engineering, not generation.

**Level 4:** “Generate a clean vocal track based on the whole song.”

Lyrics, melody, phrasing, emotion, refinement at the word and timing level. Like adding a major feature to a large codebase. One-shot attempts will fail. But with a human-in-the-loop, reviewing, steering, refining, this becomes feasible. The AI drafts, the human produces.

**Level 5:** “Give me fire.”

This must fail.

Just like “make me Facebook” in coding, the spec is undefined. Taste *is* the task. Neither humans nor AI can guarantee this.

---

## Who could even make this

Building a brand-new DAW? Dead end. Producers are deeply locked into their tools. Switching DAWs isn’t like switching editors, it means relearning muscle memory, mental models, creative habits. For many producers, it’s practically impossible.

So any Cursor moment has to either sit on top of existing DAWs or deeply integrate with them.

**Splice** has an interesting edge. Not DAW engineering, but data. Cross-DAW usage, massive libraries, user behavior at scale. Its natural position is as an intelligence layer, something like an LLM for music production that other tools call into.

**Apple and Logic Pro** already ship features that hint at an agentic future. Session players that suggest MIDI, react to reference audio, generate parts from scratch. Apple has vertical integration: hardware, OS, DAW. It can ship something real. But it’s also closed. A Cursor-like ecosystem thrives on extensibility, not just polished features.

**Ableton Live** is interesting for a specific reason: its project files are XML-based. That means sessions are structured, serializable, writable. In principle, an AI can already read and modify a Live project the way a coding agent reads and edits source files.

The blocker isn’t the format. It’s the model.

The IDE substrate exists. What’s missing is a music-production-focused base model that understands intent, taste, and workflow, not just structure.

---

## Why this hasn’t happened yet

Three hard blockers.

**No clear “text of music.”** Code has text. Serializable, diffable, composable. That’s why LLMs worked so well, so fast. Music doesn’t have a single equivalent. MIDI is editable but incomplete. Audio is complete but opaque. Without a clear fundamental representation, everything above it becomes fragile.

**No production-native base model.** Cursor didn’t invent intelligence. It orchestrated a strong base model. There’s no equivalent yet for music production, one that understands MIDI, audio, arrangement, plugins, mixing, and taste as a unified domain. Current models generate outputs, they don’t reason inside workflows.

**Locked ecosystems.** There’s no VS Code-level DAW that is open and dominant. DAWs are closed, fragmented, deeply personal. That pushes AI to the plugin layer, where integration is safer and optional.

That’s why we see “AI inside plugins” everywhere, and almost nowhere at the DAW core.

---

## Where we are now

Here’s something I made in 30 minutes, mostly Splice samples:

JavaScript is not enabled, using default audio player instead.

![Ableton Live project screenshot](https://blog-r2.jw1.dev/Xnip2026-01-08_17-26-22.webp)

This is probably the fastest way to get something production-ready with a traditional workflow. But let’s be honest, the vibe factor is low. It’s assembled, not created.

What’s not AI here? Pretty much everything that matters. Sidechain, dialed in by hand. Reverb, tweaked until it sat right. Arrangement, mixing, mastering, all me.

And what kind of AI do we have now in music production?

Splice. It uses AI to find sounds faster, made matching tones easier. Real gains, but still operating inside the traditional production phase.

Plugins. Pitch correction, noise reduction, vocal tuning, loudness matching. These are genuinely useful. They save time. But they don’t change how music is made, they just speed up tasks inside the same old workflow.

Then there are the full-track generators. Suno, Udio, you name it. They can spit out complete songs, sometimes with lyrics, and honestly the results are surprisingly not bad. For background music, promotional videos, that kind of stuff? They work. Fast, cheap, good enough.

But at this point they skip something critical: **production**.

### The paradox

Prompt → final audio. No MIDI, no arrangement control, no micro-timing, no note-level editing. You get output, not a workspace. It’s closer to collage than composition.

Yes, I know Suno has its own studio app, the thing is, if Suno really wants to enter serious production, it needs fine-grained control. How much sidechain pump fits my taste? How hard should the compressor hit before the vocal sounds perfect? What’s the right master loudness? Should I apply true peak limiting?

But the moment you add those controls, you’re building a DAW. And those controls still require professional knowledge to use well. A beginner and a seasoned producer using the same AI tool will produce vastly different results. Just like a junior dev and a senior dev using Cursor. The tool accelerates. It doesn’t replace judgment.

### The taste

In code, taste is often invisible to users. A shitty function and a beautifully designed one can produce the same result: it works. Architecture and elegance mostly matter to developers.

Music doesn’t work like that.

Humans are extremely sensitive to sound. Timing, tone, balance, texture, these are immediately perceptible. Choose a bad string library? Listener knows instantly. There’s no abstraction layer that hides bad taste.

And there’s no “ship now, fix later” model. Software can be patched. Music can’t. Once it’s released, it’s frozen. The first version is the version.

### The moving target

There’s something even deeper. Code is functional, music is cultural. Assembly from 1950 still runs correctly today. Music from 1950? Technically fine, culturally dated. Code has a stable target: correctness. Music’s target drifts with time, with generations, with vibes no one can fully articulate.

AI learning code is learning “what works.” AI learning music is learning “what felt good to people in the past.” But taste keeps moving. Training data is always yesterday. The ground truth itself is in motion.

That’s why skipping the production phase works for low-stakes content, but fails for anything serious.

---

## So when?

Not when ...