---
name: worked-example
description: >
  Walk through a fully solved problem with explicit "I do / we do /
  you do" scaffolding so the learner sees an expert's chain of
  reasoning before being asked to reproduce it. Use when the learner
  says "show me how", "walk me through an example", or for any
  procedural / quantitative sub-topic.
phase: 3
triggers:
  - "show me how"
  - "walk me through an example"
  - "I do / we do / you do"
  - learner is starting a procedural or quantitative sub-topic for the first time
inputs:
  - "topic-config.md: §3 (sub-topic), §6 (verification level), §7 (preferences)"
  - "progress/<tracker>: §\"Per-Domain Detail\" for the sub-topic"
outputs:
  - "session-notes: appends a \"Worked Example\" block with all three rounds (I/We/You)"
  - "tracker: may mark the sub-topic `[~]` In Progress; may close gaps on success of the \"you do\" round"
verification: inherits
---

## Purpose

Direct instruction works best when the learner sees a worked solution
*before* attempting one alone (Sweller's worked-example effect). This
skill formalizes the I-do / we-do / you-do progression in three rounds,
then hands the result to the tracker.

## When to use

- Procedural or quantitative sub-topics (formulas, derivations,
  calculations, multi-step decisions).
- Learner explicitly asks for an example.
- After a `diagnose-baseline` shows the learner has the concept but
  not the procedure.

## When NOT to use

- Pure conceptual sub-topics — use plain Socratic explanation or
  `analogy-bank`.
- Learner just wants a quick definition — don't drag them through
  three rounds.

## Procedure

1. **Pick a representative problem** at the lower end of typical
   difficulty for the sub-topic. State the problem clearly.
2. **Round 1 — "I do".** Solve it end-to-end out loud, narrating each
   decision: *"Step 1, I check whether … because …. Step 2, …."*
   Highlight 2–3 places novices most often slip.
3. **Round 2 — "We do".** Pose a near-identical problem (same
   structure, different numbers/names). Solve it together: ask the
   learner what comes next at each step; supply the answer if they're
   stuck within ~10 seconds.
4. **Round 3 — "You do".** Pose a third problem with a small twist
   (one extra constraint, or a transposed step). Stay silent until
   they finish, then mark each step right/wrong/partial.
5. Score the "you do" round: clean solo solution → mastery candidate;
   needed prompts → still in progress; failed → leave a clear gap.
6. **Tracker write-back** — see below.

## Verification policy hook

Under `strict`, every formula, threshold, or rule used in the worked
example must be cited from authoritative material listed in
`topic-config.md` §4. Under `standard`/`relaxed`, citations are
required only for specific numerical claims.

## Tracker write-back

- **session-notes** — append a `### Worked Example — {{sub-topic}}`
  block containing all three problems, the worked solutions for
  rounds 1–2, and the learner's solo attempt + score for round 3.
- **tracker** —
  - Move the sub-topic from `[ ]` → `[~]` (or keep `[~]`).
  - If the "you do" round was clean, mark `[x]` with confidence
    Medium-High and note the worked example in *Key points*.
  - If the learner failed at a specific step, open or update a
    Medium-severity gap naming that step.

## Example

> **Coach (skill: `worked-example`):**
>
> *Round 1 (I do):* "Problem: client age 62, $200k IRA, wants $30k now,
> tax bracket 24%. Step 1, check whether the 10% penalty applies …."
>
> *Round 2 (we do):* "Same setup, $250k IRA, $40k withdrawal. What's
> step 1?"
>
> *Round 3 (you do):* "Same setup, but client is 58 and the
> withdrawal is for qualifying medical expenses. Solve it solo."
>
> *(learner answers; coach scores; updates tracker.)*
