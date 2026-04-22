# Study-System — A Topic-Agnostic Guided-Learning Repo

A minimal, file-based learning system you can point at **any** study topic —
an exam, a certification, a language, a framework, a textbook — and turn an
AI assistant (Claude Code, etc.) into a patient, Socratic study coach that
**remembers your progress across sessions**.

Inspired by [karpathy's gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
on using AI as a learning partner. This repo started as a CFP-exam-only
study log (and the original learner [passed the exam in November 2025 🎉](#case-study-cfp-exam-pass)).
It has since been refactored into a topic-agnostic template — the CFP files
are kept in place as a working **case study** of what the system looks like
after a few weeks of real use.

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
topic-config.md              ← what you are studying (you fill this in)
topic-config.template.md     ← blank template to copy from
INIT.md                      ← the init + daily-update workflows
CLAUDE.md                    ← coach behavior (Socratic, two-step tracking)

progress/
  STUDY-TRACKER-TEMPLATE.md  ← generic tracker template
  cfp-study-tracker.md       ← legacy CFP tracker (case study)

sessions/
  SESSION-TEMPLATE.md        ← generic per-session note template
  YYYY-MM-DD/
    session-notes.md         ← one folder per study day
```

---

## Quick Start (new topic)

1. **Clone** this repo (or use it as a template).
2. _(Optional)_ Clear the CFP case-study data:
   ```bash
   rm -rf progress/cfp-study-tracker.md sessions/2025-*
   ```
3. **Configure your topic**:
   ```bash
   cp topic-config.template.md topic-config.md
   # edit topic-config.md: topic name, target date, domains + weights,
   # materials, verification level, personalization
   ```
4. **Open Claude Code in the repo** and say:
   > "Initialize the tracker."

   The coach will read `topic-config.md`, generate
   `progress/<short-code>-study-tracker.md` from the template, and propose
   the first study session based on the highest-weight, lowest-coverage
   domain.
5. **Just start asking questions.** After each session, the coach writes
   a session notes file under `/sessions/YYYY-MM-DD/` and updates the
   tracker — no manual bookkeeping required.

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
cp topic-config.md topic-config.cfp.bak.md   # save the old config
cp topic-config.template.md topic-config.md  # blank it out
# fill in your new topic, then ask the coach to "initialize the tracker"
```

A new `progress/<new-short-code>-study-tracker.md` is generated; the old
tracker stays in `progress/` as historical reference.

---

## Migration Note (from the CFP-only version)

If you knew this repo as the CFP study repo, here's what changed:

- **`CLAUDE.md`** is no longer CFP-specific. The CFP domain list has moved
  into `topic-config.md`.
- **`topic-config.md`** is new — it's the only file that knows about your
  topic.
- **`progress/STUDY-TRACKER-TEMPLATE.md`** is the new generic tracker.
- **`progress/cfp-study-tracker.md`** is preserved verbatim as a worked
  example of what a populated tracker looks like.
- **`sessions/SESSION-TEMPLATE.md`** has been generalized (no exam-specific
  fields) and now has a standard "new gaps / resolved gaps" section.
- **All historical session notes are kept** under `sessions/2025-*/`.

Nothing is lost; the CFP files now play the role of "an example topic
already in use".

---

## Case Study: CFP Exam Pass

The original learner (chenran) used this repo to prepare for the
**Certified Financial Planner (CFP) exam** after a previous failed attempt
in November 2024. Across 23 sessions (Oct 11 – Nov 7, 2025), they reached
**82% mastery (60/73 sub-topics)** and **passed the CFP exam on
November 10, 2025**.

The CFP `topic-config.md`, `progress/cfp-study-tracker.md`, and
`sessions/2025-*` are kept in this repo as a real-world demonstration of
what the system looks like after several weeks of disciplined use.

Connect with the original author: [LinkedIn](https://linkedin.com/in/chenran818) ·
[Twitter / X](https://x.com/chenran818) ·
[知乎](https://www.zhihu.com/people/chenran)

---

## Free Supplementary Resources (for CFP specifically)

If you happen to be using this repo for CFP prep, these free resources are
nice complements to the workflow above:

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
