# Study-System — A Topic-Agnostic Guided-Learning Repo

[中文说明](./README.zh-CN.md)

A minimal, file-based learning system you can point at **any** study topic —
an exam, a certification, a language, a framework, a textbook — and turn an
AI assistant (Claude Code, etc.) into a patient, Socratic study coach that
**remembers your progress across sessions**.

Inspired by [karpathy's gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
on using AI as a learning partner. The repo originated as a CFP-exam study
log (the original learner passed the exam in November 2025 🎉) and has
since been refactored into a clean, topic-agnostic template — clone it,
start chatting with your coding agent, and the coach will set everything
up for you.

---

## What the system does

For any topic you choose, the coach will:

- 🧠 **Teach Socratically** — ask what you know first, then explain in
  ~200-word chunks, then check understanding before moving on.
- 📒 **Log every session** — one detailed `session-notes.md` per study day.
- 📈 **Maintain a single tracker** — `progress/<topic>-study-tracker.md` is
  the always-current source of truth for mastered sub-topics, open
  knowledge gaps, and what to study next.
- 🎯 **Re-prioritize automatically** — domain weights × coverage × open gaps
  → the next-step study plan after every session.
- 🔍 **Cite or admit uncertainty** — strictness is configurable per topic
  (strict / standard / relaxed).

---

## Repo Layout

```
topic-config.md              ← what you are studying (blank template; you fill this in)
topic-config.template.md     ← reference copy of the same template
INIT.md                      ← the init + daily-update workflows
CLAUDE.md                    ← coach behavior (Socratic, two-step tracking)
AGENTS.md                    ← same instructions for Codex / opencode / other agents

progress/
  STUDY-TRACKER-TEMPLATE.md  ← generic tracker template (the coach generates
                               progress/<short-code>-study-tracker.md from this)

sessions/
  SESSION-TEMPLATE.md        ← generic per-session note template
  YYYY-MM-DD/                ← one folder per study day (created by the coach)
    session-notes.md
```

---

## Quick Start

1. **Clone** this repo (or use it as a template).
2. **Open it in your coding agent of choice** — Claude Code, Codex, opencode,
   etc. — and just start chatting. Say something like:
   > "I want to study X. Initialize the tracker."

   The coach reads `CLAUDE.md` (or `AGENTS.md`), notices that
   `topic-config.md` is still in template state, and walks you through
   filling it in (topic name, target date, domains + weights, materials,
   verification level, personalization). It then generates
   `progress/<short-code>-study-tracker.md` from the template and proposes
   the first study session based on the highest-weight, lowest-coverage
   domain.
3. **Just keep asking questions.** After each session, the coach writes a
   session-notes file under `/sessions/YYYY-MM-DD/` and updates the tracker
   — no manual bookkeeping required.

> **Tip:** You can also fill in `topic-config.md` by hand first and then
> ask the coach to "initialize the tracker" — both flows work.

### Compatibility with `/init`

Most coding agents have an `/init` command that scaffolds an instructions
file for the project (`CLAUDE.md` for Claude Code, `AGENTS.md` for Codex
and opencode). This repo already ships both files, so:

- You **don't need to run `/init`** — the agent will pick up the existing
  instructions automatically.
- If you do run `/init`, please keep `CLAUDE.md` / `AGENTS.md` intact (or
  merge, don't overwrite) so the study-coach behavior is preserved.

Full step-by-step is in [`INIT.md`](./INIT.md).

---

## Daily Loop

| When | What you do | What the coach does |
|------|-------------|---------------------|
| Start of session | Ask a question, request practice, or say "what should I study today?" | Reads `topic-config.md`, the tracker, and the latest session notes for context |
| During | Answer comprehension checks honestly — including "I don't know" | Teaches Socratically; cites sources at the strictness level your config requires |
| End of session | Nothing — just stop | Writes today's `session-notes.md`, updates the tracker (mastered / gaps / next steps) |

Common things to ask between sessions:
- "What should I focus on today?"
- "Quiz me on my weak areas."
- "Show me my progress."
- "Re-prioritize based on my exam date."

---

## Switching Topics

Because all behavior is driven by `topic-config.md`, switching topics is
just swapping that one file:

```bash
cp topic-config.md topic-config.<old-topic>.bak.md   # save the old config
cp topic-config.template.md topic-config.md          # blank it out
# fill in your new topic, then ask the coach to "initialize the tracker"
```

A new `progress/<new-short-code>-study-tracker.md` is generated; any
previous tracker stays in `progress/` as historical reference.

---

## Origin: CFP Exam Case Study

This system was originally built and battle-tested by the original learner
(chenran) preparing for the **Certified Financial Planner (CFP) exam**.
Across 23 sessions (Oct 11 – Nov 7, 2025) they reached 82% mastery
(60/73 sub-topics) and **passed the CFP exam on November 10, 2025**.

The CFP-specific tracker and session notes have been removed so the repo
ships as a clean template, but the workflow, file structure, and coach
behavior are exactly what was used in that successful run.

Connect with the original author: [LinkedIn](https://linkedin.com/in/chenran818) ·
[Twitter / X](https://x.com/chenran818) ·
[知乎](https://www.zhihu.com/people/chenran)

If you happen to be using this repo for CFP prep, these free resources
pair nicely with the workflow:

- [Open Exam Prep Podcast](https://open.spotify.com/show/55EmWfdtPaK641q4Rk3mI1)
- [Open Exam Prep YouTube](https://www.youtube.com/@Open-exam-prep)
- [open-exam-prep.com](https://open-exam-prep.com/)
- [Financial Planning Essentials playlist](https://open.spotify.com/playlist/6GUIZvnpaiOiYmXkanqwZ8)

For other topics, list your own materials in `topic-config.md` §4 — the
coach will prefer to cite them.

---

## Philosophy

- **Conversational and judgment-free.** It's safe to not know things.
- **Build on what you already know.** Every explanation starts with your
  baseline.
- **Check understanding before moving on.** No silent skipping.
- **Adapt to the learner.** Different analogies, examples, granularity.
- **Deep understanding > memorization.** Aim for skills you keep after the
  exam is over.
