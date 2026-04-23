---
name: mixed-practice
description: >
  Run an interleaved practice block that mixes problems from multiple
  sub-topics or domains in a single session, instead of blocking by
  topic. Use when the learner says "mixed practice", "interleave me",
  "give me a random set", or when several sub-topics are simultaneously
  in progress.
phase: 5
triggers:
  - "mixed practice"
  - "interleave"
  - "random set"
  - 3+ sub-topics are simultaneously `[~]` In Progress
inputs:
  - "topic-config.md: §3 (domains, weights)"
  - "progress/<tracker>: §\"Per-Domain Detail\" (in-progress + mastered), §\"Knowledge Gaps\""
outputs:
  - "session-notes: appends a \"Mixed Practice\" block (items, answers, scores by sub-topic)"
  - "tracker: updates per-sub-topic confidence; may close or open gaps; may inform `schedule-spaced-reviews` of misses"
verification: inherits
---

## Purpose

Interleaving (Bjork's "desirable difficulties") improves discrimination
between similar problem types and produces better transfer than blocked
practice — at the cost of feeling harder in the moment. Coach-core
naturally drifts toward blocked practice (one explanation → check →
next concept). This skill explicitly *unblocks* it.

## When to use

- The learner has 3+ active sub-topics and is starting to confuse
  them.
- 1–2 weeks before a mock or real exam.
- Whenever the learner asks for a random set.

## When NOT to use

- Brand-new sub-topic with no other peers — there's nothing to
  interleave with; use `worked-example` first.
- The learner is fragile or low on energy — interleaving is meant to
  feel hard. Switch to `retrieval-quiz` on a single sub-topic instead.

## Procedure

1. **Pick the pool.** Choose 2–4 sub-topics that are commonly
   confused (same domain, similar surface features) or that share
   procedures.
2. **Build the set.** Generate 6–10 problems and **shuffle** them so
   no two consecutive items are from the same sub-topic.
3. **Add 1–2 surface-similar / structurally-different items** that
   tempt the learner into the wrong procedure (the discrimination
   payoff of interleaving comes from these).
4. **Run it.** One item at a time. After each, ask the learner to
   *first name the sub-topic* it belongs to, *then* solve it. Naming
   is the discrimination signal.
5. **Score.** Track two scores per sub-topic: discrimination
   (named correctly?) and execution (solved correctly?). They diagnose
   different problems.
6. Debrief: highlight any pair the learner repeatedly confused, and
   open or update a gap on the *distinction* itself (not on either
   sub-topic alone).
7. **Tracker write-back** — see below.

## Verification policy hook

Under `strict`, all "correct" answers in the debrief must be cited.
Discrimination feedback (which sub-topic an item belongs to) does not
require citation but should reference the sub-topic codes from
`topic-config.md`.

## Tracker write-back

- **session-notes** — append a `### Mixed Practice` block with the
  pool, the shuffled items, both score columns (discrimination /
  execution), and the debrief.
- **tracker** —
  - Per sub-topic, update confidence based on execution score.
  - Add a `Confusion: {{A}} ↔ {{B}}` gap to `🟡 Medium Severity` if
    the learner missed the discrimination on 2+ items between the
    same pair.
  - For repeat ✅ on previously `[~]` items, consider promoting to
    `[x]`.

## Example

> **Coach (skill: `mixed-practice`):** "Pool: A.2, B.4, F.51. 8 items
> shuffled. For each, name the sub-topic *first*, then solve.
>
> 1. ‘A 64-year-old wants $30k from her IRA…' — sub-topic? Then solve.
> 2. …
>
> *(scores after: discrimination 7/8, execution 6/8. The miss pattern
> shows F.51 ↔ B.4 confusion → opens a Medium gap on the distinction.)*
