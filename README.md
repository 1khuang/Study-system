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
- 📒 **Log every session** — one detailed
  `topics/<topic>/sessions/YYYY-MM-DD.md` per study day, per topic.
- 📈 **Maintain a single tracker per topic** — `topics/<topic>/tracker.md`
  is the always-current source of truth for mastered sub-topics, open
  knowledge gaps, and what to study next.
- 📝 **Build chapter notes as it teaches** — definitions, numbered-point
  lists, formulas, and rules are distilled into
  `topics/<topic>/notes/<domain>-<slug>.md` (one file per chapter,
  organized by syllabus) so the learner has a clean reference to re-read
  later, separate from the chronological session log.
- 🎯 **Re-prioritize automatically** — domain weights × coverage × open gaps
  → the next-step study plan after every session.
- 🔍 **Cite or admit uncertainty** — strictness is configurable per topic
  (strict / standard / relaxed).
- 🔀 **Run multiple topics in parallel** — every topic has its own home
  under `topics/<short-code>/`, so studying CFP in the morning and AWS
  in the afternoon produces two clean files, not one tangled one.
- 🔗 **Capture cross-topic insights** — when two topics suddenly
  connect, the coach records that as a first-class file under
  `crosslinks/`, back-referenced from each topic's tracker.

---

## Repo Layout

```
.active-topic                  ← single line: <short-code> of the topic
                                 the coach defaults to in this chat
                                 (created on first init)

topic-config.template.md       ← reference template; copy it to
                                 topics/<short-code>/topic-config.md
                                 for each new topic
topic-config.md                ← legacy single-topic placeholder
                                 (kept for backwards compatibility)

INIT.md                        ← add-a-topic + daily-update workflows
CLAUDE.md                      ← coach behavior (Socratic, two-step
                                 tracking, multi-topic, crosslinks)
AGENTS.md                      ← same instructions for Codex / opencode
                                 / other agents

topics/                        ← one home per topic
  README.md                    ← explains the per-topic layout
  <short-code>/                ← e.g. cfp/, aws-saa/, linalg/
    topic-config.md            ← what this topic is
    tracker.md                 ← progress for this topic
    sessions/
      YYYY-MM-DD.md            ← one note file per study day, per topic
    notes/
      <domain>-<slug>.md       ← one chapter notes file per domain
                                 (distilled definitions, point-lists,
                                 formulas, rules — see notes/README.md)

crosslinks/                    ← cross-topic insights
  README.md
  INDEX.md                     ← reverse-chronological index
  CROSSLINK-TEMPLATE.md
  YYYY-MM-DD-<slug>.md         ← one file per insight (created on demand)

progress/
  STUDY-TRACKER-TEMPLATE.md    ← generic tracker template (the coach
                                 generates each topic's tracker.md from
                                 this)

sessions/
  SESSION-TEMPLATE.md          ← generic per-session note template
                                 (each topic's daily note is a copy)

notes/
  NOTES-TEMPLATE.md            ← generic per-chapter notes template
                                 (each topic's notes/<domain>-<slug>.md
                                 file is a copy)
  README.md                    ← what chapter notes are, when to update
```

---

## Quick Start

1. **Clone** this repo (or use it as a template).
2. **Open it in your coding agent of choice** — Claude Code, Codex, opencode,
   etc. — and just start chatting. Say something like:
   > "I want to study X. Add it as a new topic."

   The coach reads `CLAUDE.md` (or `AGENTS.md`), notices that `topics/`
   is empty, picks a `<short-code>` (or asks you for one), and walks
   you through filling in `topics/<short-code>/topic-config.md`
   (topic name, target date, domains + weights, materials,
   verification level, personalization). It then generates
   `topics/<short-code>/tracker.md` from the template, sets
   `.active-topic` to the new short-code, and proposes the first
   study session based on the highest-weight, lowest-coverage domain.
3. **Just keep asking questions.** After each session, the coach writes a
   session-notes file at `topics/<short-code>/sessions/YYYY-MM-DD.md`
   and updates that topic's tracker — no manual bookkeeping required.

> **Tip:** You can also fill in `topics/<short-code>/topic-config.md`
> by hand first (copy from `topic-config.template.md`) and then ask
> the coach to "initialize the tracker" — both flows work.

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
| Start of session | Ask a question, request practice, or say "what should I study today?" | Reads `.active-topic`, then that topic's `topic-config.md`, `tracker.md`, and most recent session notes for context |
| During | Answer comprehension checks honestly — including "I don't know" | Teaches Socratically; cites sources at the strictness level your config requires |
| Switch topic mid-chat | Say "switch to <short-code>" | Rewrites `.active-topic`, announces the switch, re-loads the new topic |
| End of session | Nothing — just stop | Writes today's `topics/<short-code>/sessions/YYYY-MM-DD.md`, updates that topic's tracker (mastered / gaps / next steps), and creates a `crosslinks/` file if the session produced a cross-topic insight |

Common things to ask between sessions:
- "What should I focus on today?"
- "Quiz me on my weak areas."
- "Show me my progress."
- "Re-prioritize based on my exam date."
- "Switch to <other-topic>."
- "Add a new topic: <name>."
- "Show me all my crosslinks."

---

## Multiple Topics & Cross-topic Insights

This repo is designed to host **many topics in parallel** — an exam,
a side framework, a language, all under the same git history. Two
mechanisms make that work:

### Per-topic homes

Each topic lives entirely under `topics/<short-code>/`:

```
topics/cfp/topic-config.md
topics/cfp/tracker.md
topics/cfp/sessions/2026-04-23.md

topics/aws-saa/topic-config.md
topics/aws-saa/tracker.md
topics/aws-saa/sessions/2026-04-23.md
```

Studying two topics on the same day produces two independent files —
no collisions, no mingled trackers. The repo-root file `.active-topic`
holds a single line (the active topic's `<short-code>`) so the coach
knows which topic each chat defaults to. Switching is one sentence:
_"switch to aws-saa"_ rewrites `.active-topic` and the coach announces
the switch.

### Crosslinks

When two topics suddenly connect ("oh, this AWS IAM idea is the same
shape as that CFP trust-permissions concept"), the coach records the
moment as a first-class file under `crosslinks/`:

- `crosslinks/INDEX.md` — reverse-chronological global index
- `crosslinks/YYYY-MM-DD-<slug>.md` — one file per insight, with
  YAML front-matter pinning every participating `topics:` and
  sub-topic `links:`
- Each participating topic's `tracker.md` gets a back-reference row
  in its **Cross-topic Links** section, so the insight is reachable
  from either side.

Full coach behavior is in [`CLAUDE.md`](./CLAUDE.md) §9; file
conventions are in [`crosslinks/README.md`](./crosslinks/README.md).

---

## Switching or Adding Topics

To add another topic at any time, just say in chat:

> "Add a new topic: <name>."

The coach picks (or asks for) a `<short-code>`, creates
`topics/<short-code>/`, walks you through `topic-config.md`, generates
the tracker, and updates `.active-topic`.

To switch which topic the next message defaults to:

> "Switch to <short-code>."

To remove a topic, just `git rm -r topics/<short-code>/` and (if it was
active) update `.active-topic`. Nothing else has to change; other
topics and the global `crosslinks/INDEX.md` survive untouched
(crosslink files that referenced the deleted topic remain as historical
artifacts; you may prune them if desired).

### Legacy single-topic repos

Repos that started before the multi-topic layout existed kept their
config at `topic-config.md` in the repo root and their tracker at
`progress/<short-code>-study-tracker.md`. That layout still works —
the coach detects it automatically and uses it. To migrate, see
[`INIT.md`](./INIT.md) §3.

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
