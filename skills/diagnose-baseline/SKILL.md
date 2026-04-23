---
name: diagnose-baseline
description: >
  Run a focused pre-assessment interview before teaching a new sub-topic
  so explanations can start at the right level. Use when entering a
  brand-new sub-topic, when the learner says "I think I sort of know X"
  or "test what I know about X", or after a re-init when prior knowledge
  needs re-calibration.
phase: 1
triggers:
  - "what do I already know about"
  - "test what I know about"
  - "diagnose me on"
  - learner is about to start a sub-topic with no prior coverage in the tracker
  - re-init detected and prior-knowledge field is empty in topic-config.md
inputs:
  - "topic-config.md: §1 (topic), §3 (target sub-topic), §7 (prior knowledge)"
  - "progress/<tracker>: §\"Per-Domain Detail\" for the target sub-topic"
outputs:
  - "session-notes: appends a \"Diagnostic Baseline\" block under \"Topics Covered\""
  - "tracker: may flip a sub-topic from `[ ]` to `[~]`, may add gaps, updates §7 personalization notes if the learner asks"
verification: inherits
---

## Purpose

Coach-core already opens with "what do you already know about X?", but
that single line often surfaces only the top of the iceberg. This skill
runs a tight 3–5 question diagnostic that maps prior knowledge,
misconceptions, and confidence *before* any explanation is given. The
output seeds the tracker so the next sessions don't waste time
re-teaching things the learner already owns.

## When to use

- Starting a fully new sub-topic (`[ ]` in the tracker).
- Learner explicitly asks to be tested on what they know.
- After topic re-init when `Personalization → Prior Knowledge` is empty
  or stale.

## When NOT to use

- Inside an active explanation (use the default comprehension check).
- For a sub-topic already marked `[~]` or `[x]` — use
  `retrieval-quiz` instead.
- When the learner just wants the answer — respect the request and skip
  diagnosis.

## Procedure

1. Confirm the target sub-topic by name and code (e.g. `B.4 …`).
2. Ask **one** broad opener: *"In your own words, what is {{sub-topic}}?"*
3. Based on the answer, ask 2–3 progressively narrower questions that
   probe the most likely misconceptions (look up the sub-topic in the
   tracker for any noted gaps).
4. Ask one **calibration** question: *"On a 1–5 scale, how confident are
   you in that answer?"* Note the gap between confidence and accuracy.
5. Summarize back: *"Here's what you already have solid: …. Here's what
   we should focus on: …."*
6. Offer the learner the choice: dive into the weakest piece now, or
   schedule it for later.
7. **Tracker write-back** — see below.

## Verification policy hook

This skill makes few factual claims itself; it mostly evaluates the
learner's claims. Under `strict`, any correction the coach offers
during the wrap-up must cite an authoritative source. Under
`standard`/`relaxed`, corrections may be uncited but must be flagged
when uncertain.

## Tracker write-back

- **session-notes** — append a `### Diagnostic Baseline — {{sub-topic}}`
  block listing the questions asked, the learner's answers, the
  confidence rating, and the summarized "solid vs gap" map.
- **tracker** —
  - Flip the sub-topic from `[ ]` to `[~]` under `🟡 In Progress`.
  - For each misconception caught, add a row to **Knowledge Gaps**
    (severity = High if it would block downstream sub-topics; else
    Medium).
  - If the learner volunteered new prior knowledge worth remembering
    across sessions, ask permission and append it to
    `topic-config.md` §7 *Prior Knowledge*.

## Example

> **Learner:** "Let's start on Roth conversions. I think I sort of know them."
>
> **Coach (skill: `diagnose-baseline`):** "Quick 3-question diagnostic
> first so I don't waste your time. (1) In your own words, what *is* a
> Roth conversion? … (2) Who pays the tax, and when? … (3) Why might
> someone do one even if it raises this year's tax bill?"
>
> *(learner answers; coach scores; one misconception about pro-rata
> caught)*
>
> **Coach:** "Solid: definition, timing of tax. Gap: the pro-rata rule
> when there are pre-tax IRA balances. Want to tackle the pro-rata rule
> now, or schedule it? I'll log this either way."
