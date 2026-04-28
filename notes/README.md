# `notes/` — Source Template for Per-Topic Chapter Notes

This directory holds **only the template** used to seed each topic's
own `topics/<short-code>/notes/` directory. Real notes never live here;
they live under the topic that owns them.

```
notes/
  NOTES-TEMPLATE.md       ← source template (this dir)
  README.md               ← you are here

topics/<short-code>/
  notes/                  ← actual chapter notes for this topic
    A-domain-slug.md      ← one file per domain (chapter)
    B-domain-slug.md
    …
```

---

## Why a `notes/` folder per topic?

The repo already captures two artifacts per topic:

- **`tracker.md`** — the *map*: what's mastered, what's open, what to
  study next. Indexes by sub-topic, not by content.
- **`sessions/YYYY-MM-DD.md`** — the *log*: what happened in each
  conversation, in chronological order. Hard to re-read because
  knowledge is scattered across days.

Neither of those is a clean place to look up *"what's the actual
definition of X?"* or *"what are the five conditions for Y?"* months
later. The **chapter notes** fill that gap: a deduplicated,
organized-by-syllabus reference that grows as the coach surfaces
definitive knowledge in conversation.

## What goes in (and what does not)

**In** — *definitive* knowledge the coach explicitly states:

- Definitions ("X is defined as …")
- Numbered / bulleted lists of discrete facts ("The four pillars of …")
- Formulas, rules, thresholds, exact values
- Canonical short examples worth re-reading
- Caveats and common confusions

**Out** — anything that belongs in another artifact:

- The conversation itself, comprehension checks, the learner's
  starting understanding → **session notes**
- Mastery state, gaps, next-step planning → **tracker**
- Cross-topic insights → **`crosslinks/`**

## Granularity

**One file per domain (chapter)**, named `<domain-code>-<slug>.md`
(e.g. `A-foundations.md`, `B-financing.md`). Inside the file, one
section per sub-topic, in the same order as `topic-config.md` §3, so
the chapter reads top-to-bottom and matches the syllabus.

A topic with five domains ends up with five notes files — small
enough to scan, structured enough to grow.

## When the coach updates notes

See [`../CLAUDE.md`](../CLAUDE.md) §3a for the full rules. The short
version: any time the coach states something definitive in a
conversation (a definition, a numbered list, a formula, a fixed rule),
it must also write that fact into the corresponding sub-topic section
of the matching chapter file — and record which file(s) it touched in
the session note's "Chapter Notes Updated" section.

## Initialization

When a new topic is added (`INIT.md` §1.2), the coach creates
`topics/<short-code>/notes/` with one seed file per domain (copied from
this directory's `NOTES-TEMPLATE.md`, headers filled in, sub-topic
sections present but marked `_No notes captured yet._`). Files
populate as the learner studies.

## Verification policy

Notes inherit the topic's verification level from `topic-config.md`.
For `strict` topics, every concrete fact (number, threshold, rule,
date) written into a notes file must carry a source citation, exactly
as it would in a session note. If a previously-stored fact is later
corrected, the coach edits the existing entry **in place** and adds a
short "Corrected on YYYY-MM-DD: was X, now Y (source)" line so the
history is preserved without polluting the body.
