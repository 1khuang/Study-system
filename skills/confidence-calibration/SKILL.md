---
name: confidence-calibration
description: >
  Track the gap between the learner's stated confidence and their
  actual correctness across recent items, then surface
  over-/under-confidence patterns and adjust study priorities. Use
  when the learner says "am I overconfident", "calibrate me",
  "calibration check", or periodically (e.g. after every 20 quiz
  items).
phase: 7
triggers:
  - "calibrate me"
  - "am I overconfident"
  - "confidence check"
  - 20+ quiz/practice items have been logged since the last calibration
inputs:
  - "topic-config.md: §3 (domains)"
  - "progress/<tracker>: §\"Per-Domain Detail\" (confidence labels), §\"Calibration Log\" (created on first use), recent session-notes quiz/exam scores with confidence ratings"
outputs:
  - "tracker: maintains §\"Calibration Log\" and updates per-sub-topic confidence labels where they're systematically off"
  - "session-notes: appends a \"Calibration\" block"
verification: inherits
---

## Purpose

Learners (and coaches) systematically over-rate familiarity for
material they've recently *seen* but not recently *retrieved*
(Dunning-Kruger / Bjork's storage-vs-retrieval distinction). This
skill explicitly compares stated confidence to outcomes and writes
the trend into the tracker so over-confident topics get more practice
and under-confident topics stop getting redundant re-teaching.

## When to use

- Periodically — every ~20 quiz/practice items, or weekly.
- Learner asks to be calibrated.
- Mock-exam debrief shows score patterns that contradict the
  tracker's confidence labels.

## When NOT to use

- Fewer than ~10 logged items since the last calibration — too noisy.
- Right after a single bad question — don't over-react.

## Procedure

1. **Pull the data.** Walk recent session-notes (last 1–2 weeks of
   quiz/practice/exam blocks) and assemble item-level rows:
   `sub-topic | confidence (1–5) | correct? (✅/❌)`.
2. **Compute calibration.** For each confidence level, compute the
   accuracy rate. A well-calibrated learner has accuracy ≈
   `confidence × 20%` (so 4/5 confidence → ~80% accuracy).
3. **Spot the pattern**:
   - **Overconfident** if confidence ≥ 4 but accuracy < 60% on those
     items.
   - **Underconfident** if confidence ≤ 2 but accuracy > 70%.
   - **Mixed** patterns at the sub-topic level matter most.
4. **Discuss with the learner.** Show them the table; ask them to
   self-attribute (was it wishful thinking? recency illusion? a
   single hard sub-topic dragging the average?).
5. **Adjust the tracker.** Per overconfident sub-topic: re-label
   confidence and queue a `retrieval-quiz`. Per underconfident
   sub-topic: re-label and *de-prioritize* further re-teaching.
6. **Tracker write-back** — see below.

## Verification policy hook

This skill makes statistical claims about the learner's own data, not
about the topic. Verification policy mostly doesn't bind, but the
table must be derivable from the session-notes (no fabricated rows).

## Tracker write-back

- **tracker** —
  - Maintain `## Calibration Log` with rows
    `Date | Items | Overall accuracy | Avg confidence | Bias
    (over/well/under) | Notes`.
  - Update `Per-Domain Detail` confidence labels (`High`,
    `Medium-High`, `Medium`) for any sub-topic with a clear bias
    pattern, and note the date.
  - Feed adjusted priorities back via `replan-priorities` if the bias
    is severe.
- **session-notes** — append a `### Calibration` block with the table
  and the chosen adjustments.

## Example

> **Learner:** "Calibration check."
>
> **Coach (skill: `confidence-calibration`):** "Last 24 items:
>
> | Confidence | Items | Accuracy |
> |---|---|---|
> | 5 | 6 | 50% |
> | 4 | 9 | 67% |
> | 3 | 5 | 80% |
> | 2 | 4 | 75% |
>
> You're **overconfident at the top** — every '5' on inherited-IRA
> rules was wrong. Demoting B.10 from High to Medium-High and queuing
> a retrieval-quiz on it tomorrow. You're **slightly underconfident**
> on Domain A — stop asking me to re-teach A.2."
