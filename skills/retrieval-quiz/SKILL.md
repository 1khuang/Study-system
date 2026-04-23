---
name: retrieval-quiz
description: >
  Run a short active-recall quiz on already-introduced sub-topics —
  no re-teaching, no hints up front — to strengthen memory and surface
  decay. Use when the learner says "quiz me", "test me on X", or as
  the engine behind `schedule-spaced-reviews`.
phase: 5
triggers:
  - "quiz me"
  - "test me on"
  - "active recall"
  - invoked by `schedule-spaced-reviews` for due items
inputs:
  - "topic-config.md: §3 (current scope), §6 (verification level)"
  - "progress/<tracker>: §\"Per-Domain Detail\" for selected sub-topics, §\"Knowledge Gaps\""
outputs:
  - "session-notes: appends a \"Retrieval Quiz\" block (questions, answers, scores)"
  - "tracker: updates confidence on mastered items, adds/closes gaps, feeds back into §\"Spaced Review Queue\" if applicable"
verification: inherits
---

## Purpose

Retrieval practice (Roediger & Karpicke) consistently outperforms
re-reading and even elaborative study for long-term retention.
Coach-core has a one-question comprehension check, but no dedicated
retrieval *session*. This skill provides one — short, brisk, no
re-teaching mid-quiz.

## When to use

- Learner asks to be quizzed.
- Spaced-review item is due and a multi-question pass is wanted.
- End of a study block, before a break.

## When NOT to use

- The learner has not yet been introduced to the material (pre-test is
  fine, but use `diagnose-baseline` instead).
- The learner is mid-explanation — finish acquisition first.

## Procedure

1. **Scope it.** Either: (a) one sub-topic the learner names, or
   (b) the N highest-priority `[~]`/`[x]` items from the tracker
   (default N = 5).
2. **Format it.** Mix question types: 2 short-answer, 2 application
   ("what would you do if…"), 1 distinction ("how is X different from
   Y?"). For `strict`-policy topics, prefer the canonical exam wording.
3. **Run it.** One question at a time. **No hints, no teaching.** If
   the learner doesn't know, accept "pass" and move on.
4. **Score after, not during.** When the quiz is finished, walk
   through each item: ✅ / 🟡 / ❌, with the correct answer and a
   one-line explanation only on misses.
5. Re-quiz any ❌ items after a brief pause (within the same
   session) — this is the "successive-relearning" boost.
6. **Tracker write-back** — see below.

## Verification policy hook

Under `strict`, every "correct answer" provided in the post-quiz walk
must be cited from authoritative material. Under `standard`, cite for
numbers/dates/rules. Under `relaxed`, citations optional unless the
learner asks.

## Tracker write-back

- **session-notes** — append a `### Retrieval Quiz` block: scope,
  questions, learner answers, scores, and the re-quiz results.
- **tracker** —
  - For ✅ on a `[~]` item: consider promoting to `[x]` (Medium-High
    confidence) if multiple questions on it were answered cleanly.
  - For ❌ on a `[x]` item: demote to `[~]` and open a Medium gap.
  - For ❌ on a `[~]` item: open or update a gap; do not promote.
  - If invoked from `schedule-spaced-reviews`, hand the score back so
    the queue's `Next Due` and `Interval` get updated correctly.

## Example

> **Learner:** "Quiz me on Domain F."
>
> **Coach (skill: `retrieval-quiz`):** "5 questions, no hints, no
> teaching mid-quiz. Ready?
>
> 1. What's the trigger event for the 10% early-withdrawal penalty?
> 2. …
>
> *(learner answers all 5; coach scores after)*
>
> Results: 3✅ 1🟡 1❌. The miss was on RMD timing — re-quizzing in 2
> minutes. Logged a gap and demoted F.48 to In Progress."
