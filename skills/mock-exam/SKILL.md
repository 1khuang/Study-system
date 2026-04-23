---
name: mock-exam
description: >
  Administer a timed, exam-style assessment with mixed coverage and a
  strict no-teaching-during-the-exam rule, then run a structured
  debrief. Use when the learner says "mock exam", "practice test",
  "simulate the real thing", or when a milestone calls for one.
phase: 6
triggers:
  - "mock exam"
  - "practice test"
  - "simulate the real thing"
  - "full-length exam"
  - a milestone in topic-config.md §5 says "first full-length practice test"
inputs:
  - "topic-config.md: §1 (target date), §3 (domains + weights), §4 (materials), §5 (milestones), §6 (verification)"
  - "progress/<tracker>: §\"Domain Progress Summary\" (for coverage-weighted item allocation)"
outputs:
  - "session-notes: appends a \"Mock Exam\" block (config, results by domain, time per item)"
  - "tracker: updates confidence broadly; opens many gaps from misses; triggers a `replan-priorities` and an `error-postmortem` on misses"
verification: inherits
---

## Purpose

A mock exam is the highest-fidelity formative assessment available
mid-prep. It surfaces decay, time-management problems, and
domain-level imbalance that no single-topic check can. The skill is
deliberate about timing, item allocation, and a clean debrief loop so
the data feeds back into the tracker.

## When to use

- Exam-prep mode, ~4 weeks out and again ~1 week out.
- Milestone explicitly calls for a full-length practice test.
- Learner requests a simulation.

## When NOT to use

- Concept-mastery / language-acquisition / project-driven topics
  where there is no "exam" — use `mixed-practice` instead.
- Major coverage gaps still exist (< 50% across most domains) —
  acquire more first; a mock now mostly tests what hasn't been taught.

## Procedure

1. **Configure** with the learner: scope (full-length vs single
   domain), item count, time limit, and whether to allow notes.
   Default: full-length, items distributed by domain weight.
2. **Item allocation.** For a target N items, allocate
   `round(N × weight_d)` items to each domain `d`. Within a domain,
   sample across `[~]` and `[x]` sub-topics; bias slightly toward
   `[~]` and toward sub-topics with open gaps.
3. **Run the exam.** Strict rules: timer on, no hints, no teaching,
   no clarifying questions beyond "what is this question asking?".
   Learner may flag items for later review.
4. **Score.** Per item: ✅ / ❌, time taken, flagged?. Aggregate by
   domain.
5. **Debrief, in this order:**
   a. Headline score and per-domain score.
   b. Time-management observations (items > 1.5× median time).
   c. For every ❌, immediately invoke `error-postmortem` (one item at
      a time; don't bulk-explain).
   d. After all post-mortems, invoke `replan-priorities` with the new
      data.
6. **Tracker write-back** — see below.

## Verification policy hook

Under `strict`, every correct answer in the debrief must be cited from
authoritative material. Item wording itself should mirror the real
exam's style (`topic-config.md` §4 should list the source). Under
`standard`/`relaxed`, less ceremony; still cite numbers.

## Tracker write-back

- **session-notes** — append a `### Mock Exam` block: configuration,
  per-item table (sub-topic, ✅/❌, time, flagged), domain summary,
  and links to the post-mortem entries.
- **tracker** —
  - Update `Domain Progress Summary` mastery counts only after
    post-mortems are done (a mock-exam ❌ + post-mortem may *increase*
    mastery if the gap is resolved on the spot).
  - Open gaps for every miss not resolved in post-mortem.
  - Write a `## Mock Exam Log` row: date, scope, score, top-3 weak
    domains.
  - Trigger `replan-priorities` to rewrite `Next-Step Study Plan`.

## Example

> **Learner:** "Full-length mock exam, please."
>
> **Coach (skill: `mock-exam`):** "Configuring: 80 items, 2 hours,
> closed-book, distributed by your topic-config weights. Timer starts
> on your 'go'.
>
> *(exam runs)*
>
> Headline: 64/80 (80%). Domain breakdown: A 90%, B 60%, F 70%, …
> Time outliers: items 12, 47, 63. Now I'll walk every miss with
> `error-postmortem`, one at a time. Then we'll re-prioritize."
