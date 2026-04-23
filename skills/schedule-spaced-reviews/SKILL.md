---
name: schedule-spaced-reviews
description: >
  Build and maintain a spaced-repetition review queue for mastered
  sub-topics so they don't decay. Use when the learner says "schedule
  reviews", "what should I review today", "spaced repetition", or
  whenever a sub-topic is newly marked mastered.
phase: 2
phases_secondary: [7]
triggers:
  - "schedule reviews"
  - "spaced repetition"
  - "what should I review today"
  - "review queue"
  - a sub-topic was just moved to ✅ Mastered in the tracker
inputs:
  - "topic-config.md: §1 (target date), §6 (verification level)"
  - "progress/<tracker>: §\"Per-Domain Detail\" (mastered sub-topics with dates), §\"Spaced Review Queue\" (created on first use)"
outputs:
  - "tracker: creates / updates a `## Spaced Review Queue` section"
  - "session-notes: appends a \"Spaced Review\" block listing what was reviewed today and the new next-due dates"
verification: inherits
---

## Purpose

The default coach loop has no spaced-repetition mechanism. Without one,
mastered sub-topics decay invisibly until they show up wrong on a mock
exam. This skill maintains a queue inside the tracker (no new files) so
review timing is visible, queryable, and re-prioritized after each
review.

## When to use

- A sub-topic just moved to `✅ Mastered` — schedule its first review.
- Learner asks for review work.
- Start of a session when the queue has items past due.

## When NOT to use

- Sub-topic is still `[~] In Progress` — it doesn't belong in the
  spaced queue yet; finish acquisition first.
- Time pressure (< 7 days to exam) — switch to `mock-exam` +
  `error-postmortem` instead; spaced review is for retention, not
  cramming.

## Procedure

1. **Build/refresh the queue.** For each `[x]` sub-topic in the
   tracker, ensure there's an entry in `## Spaced Review Queue` with
   columns `Sub-topic | Mastered On | Last Reviewed | Next Due |
   Interval (days) | Confidence Trend`.
2. **Initial intervals (SM-2-lite):** first review +1 day, then +3,
   +7, +16, +35, +90. Reset to +1 if the learner fails the review.
3. **Today's queue.** Surface every item with `Next Due ≤ today`,
   ordered by how overdue it is, capped at 5 to avoid overload.
4. **Review each item** with one short retrieval question (delegate
   to `retrieval-quiz` if a multi-question check is wanted).
5. **Score the response:** *recalled cleanly* → advance interval;
   *partial* → repeat current interval; *failed* → reset to +1 and
   re-open a Medium-severity gap on that sub-topic.
6. **Tracker write-back** — see below.

## Verification policy hook

Inherits the topic's verification level for any factual content used
in the review questions. Scheduling itself is policy-neutral.

## Tracker write-back

- **tracker** —
  - Maintain `## Spaced Review Queue` with one row per mastered
    sub-topic. Update `Last Reviewed`, `Next Due`, and `Interval`
    after each review.
  - On a failed review, add a row to `## Knowledge Gaps → 🟡 Medium
    Severity` and consider demoting the sub-topic from `[x]` back to
    `[~]` if performance was poor.
- **session-notes** — append a `### Spaced Review` block: which items
  were due, which were reviewed, scores, and the new `Next Due`
  values.

## Example

> **Learner:** "What should I review today?"
>
> **Coach (skill: `schedule-spaced-reviews`):** "3 items past due:
> A.2 (overdue 4d), B.10 (2d), F.51 (today). One question each:
>
> 1. A.2 — In one sentence, what's the difference between …?
>
> *(learner answers; coach scores; advance/repeat/reset accordingly)*
>
> Updated queue: A.2 next due in 16d, B.10 in 7d, F.51 reset to +1d
> (you missed the pro-rata rule again — re-opened a gap)."
