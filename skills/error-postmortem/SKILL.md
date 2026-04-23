---
name: error-postmortem
description: >
  Diagnose a single wrong answer by tracing it back to its root cause —
  knowledge gap, misread question, slip, time pressure, or wrong
  procedure — and convert the finding into a tracked gap and a
  remediation plan. Use after any wrong answer the learner wants to
  understand, and automatically after every miss in a `mock-exam`.
phase: 6
triggers:
  - "post-mortem this"
  - "why did I get this wrong"
  - "diagnose this miss"
  - automatically invoked by `mock-exam` for each ❌
  - 2+ wrong answers in a row in any practice
inputs:
  - "topic-config.md: §6 (verification)"
  - "progress/<tracker>: §\"Per-Domain Detail\" for the implicated sub-topic, §\"Knowledge Gaps\""
outputs:
  - "session-notes: appends an \"Error Post-Mortem\" block per item"
  - "tracker: opens a precisely-scoped gap, may demote a `[x]` to `[~]`, queues a remediation step in §\"Next-Step Study Plan\""
verification: inherits
---

## Purpose

A wrong answer with no diagnosis is wasted data. This skill enforces a
short, repeatable taxonomy so misses are categorized consistently and
tracked gaps are precise enough to *close* later.

## When to use

- Every miss inside a `mock-exam`.
- Any time the learner wants to understand a wrong answer in depth.
- When the same wrong pattern shows up in two different sessions.

## When NOT to use

- Tiny slips on trivial items the learner clearly knows — avoid
  ceremony.
- Acquisition-phase confusions where the concept hasn't really been
  taught yet — finish teaching first, then see if the miss persists.

## Procedure

For one item at a time:

1. **Re-state the item** and the learner's answer back to them
   verbatim, plus the correct answer.
2. **Ask the learner to self-classify** the cause first, choosing from
   this taxonomy:
   - `K` Knowledge gap — didn't know the rule/fact.
   - `M` Misread / parsed the question wrong.
   - `P` Wrong procedure — knew the rule but applied it incorrectly.
   - `C` Confused with sibling concept (specify which).
   - `T` Time / fatigue slip — knew it under no pressure.
   - `D` Disagreement — believes the answer key is wrong.
3. **Coach validates or corrects** the classification with one
   targeted question. (E.g. for `K`: "What rule were you trying to
   apply here?")
4. **Pinpoint the precise gap.** Not "I'm bad at Domain F" — instead
   *"On RMD calculations, I divide by life expectancy at end-of-year,
   not start-of-year"*. The gap title is the one-sentence
   misconception.
5. **Decide remediation:** *fix-now* (5-min mini-lesson +
   re-attempt), *queue* (add to `Next-Step Study Plan`), or *defer*
   (low priority).
6. **Tracker write-back** — see below.

## Verification policy hook

Under `strict`, the corrected answer must be cited. The `D`
(disagreement) classification triggers a *mandatory* citation under
all policy levels — never let a learner believe an answer key is wrong
without authoritative confirmation.

## Tracker write-back

- **session-notes** — append an `### Error Post-Mortem #N` block:
  item, learner's answer, correct answer, classification, root-cause
  sentence, remediation choice.
- **tracker** —
  - Open one row in `## Knowledge Gaps` titled with the precise
    misconception (severity High for `K` on a high-weight domain;
    Medium for `P`/`C`; Low for `T`).
  - If the miss was on a `[x]` mastered sub-topic, demote to `[~]`
    and reset its spaced-review interval to +1 day.
  - For *fix-now*, append a comprehension-check note showing the gap
    was closed in-session (then the gap moves to Recently Resolved
    in the same write-back).

## Example

> **Coach (skill: `error-postmortem`):** "Item 47. You answered $7,500;
> correct is $6,500. Self-classify: K, M, P, C, T, or D?"
>
> **Learner:** "P — I used the wrong table."
>
> **Coach:** "Validating: which table did you use, and which is
> correct?" *(learner explains; coach confirms procedure error.)*
>
> "Precise gap: *‘For inherited Roth RMDs, use the single-life table
> based on beneficiary age, not Uniform Lifetime.'* Adding as Medium.
> Want to fix-now (5-min lesson + re-attempt) or queue?"
