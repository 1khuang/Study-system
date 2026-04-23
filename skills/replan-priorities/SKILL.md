---
name: replan-priorities
description: >
  Recompute and re-rank the next-step study plan on demand, using
  weights, coverage, open gaps, and time remaining. Use when the
  learner says "what should I focus on", "re-prioritize", "my exam
  date moved", or after a major event (mock-exam, re-init,
  re-weighting).
phase: 2
triggers:
  - "what should I study next"
  - "re-prioritize"
  - "what should I focus on this week"
  - "my exam date changed"
  - target date changed in topic-config.md
  - mock-exam just finished
inputs:
  - "topic-config.md: §1 (target date), §3 (weights), §5 (milestones)"
  - "progress/<tracker>: §\"Domain Progress Summary\", §\"Knowledge Gaps\", §\"Next-Step Study Plan\""
outputs:
  - "tracker: rewrites §\"Next-Step Study Plan\", may rewrite §\"Milestones\""
  - "session-notes: appends a \"Re-prioritization\" block summarizing the new top 3 and the reasoning"
verification: inherits
---

## Purpose

The coach already recomputes the plan after every session
(`INIT.md` §2 step 7). This skill exposes that recompute as an
**explicit, deeper** action the learner can request — with visible
math and a written rationale — typically after something changed
(target date moved, mock exam revealed weak domains, re-init).

## When to use

- Learner asks "what should I focus on" outside the normal end-of-session.
- Target date / weights / milestones in `topic-config.md` just changed.
- A `mock-exam` or `error-postmortem` just finished and revealed a
  shift in weak areas.
- Once a week as a "weekly review" beat.

## When NOT to use

- Inside the middle of an explanation — finish teaching first.
- For routine end-of-session updates (the standard tracker update
  already covers this).

## Procedure

1. Read the tracker's `Domain Progress Summary` and `Knowledge Gaps`.
2. For each domain, compute
   `priority = weight × (1 − coverage) + open_gap_boost`,
   where `open_gap_boost = 0.05 × #high + 0.02 × #medium`.
3. Adjust for **time remaining**: if `Days Remaining < 30`, double the
   weight on any domain still under 50% coverage.
4. Pick the top 3 sub-topics across the highest-priority domains,
   preferring sub-topics that are `[~]` (already in progress) over
   untouched ones.
5. Cross-check against `Milestones` in `topic-config.md` §5 — if any
   milestone is at risk, surface it.
6. Present the new top-3 with one-line reasoning each, and ask the
   learner to confirm or swap anything.
7. **Tracker write-back** — see below.

## Verification policy hook

This skill makes no factual claims about the topic itself, so the
verification level doesn't change behavior. Numbers used (weights,
counts) must come from the tracker, not from memory.

## Tracker write-back

- **tracker** —
  - Replace `## Next-Step Study Plan` with the new ordered list, each
    item annotated with the priority computation in plain English
    (e.g. *"weight 25% × low coverage 0.2 + 1 high-severity gap"*).
  - If a milestone is now at risk, prepend a one-line note under
    `### Milestones`.
- **session-notes** — append a `### Re-prioritization` block with the
  before/after top-3 and a one-sentence summary of why it changed.

## Example

> **Learner:** "My exam moved up by 3 weeks. Re-prioritize."
>
> **Coach (skill: `replan-priorities`):** "Pulling current state…
> Domain F is weight 30%, coverage 0.4 → priority 18. Domain B is
> weight 20%, coverage 0.2 → priority 16, plus 2 high-severity gaps
> → 16.1. With the new date, both F and B get a 2× boost.
>
> New top 3:
> 1. F.51 Distribution rules — F is highest-leverage and you have a
>    high-severity gap there.
> 2. B.10 Financing strategies — second-largest weight × low coverage.
> 3. F.48 RMDs — closes the second F-domain gap.
>
> Want me to commit this to the tracker?"
