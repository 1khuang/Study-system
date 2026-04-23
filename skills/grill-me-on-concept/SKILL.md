---
name: grill-me-on-concept
description: >
  Interview the learner relentlessly about a concept they claim to
  understand, walking down each branch of the decision/dependency tree
  until a real shared understanding is reached or a gap is exposed.
  Use when the learner says "grill me", "stress-test me on", or when
  they self-report mastery on a sub-topic flagged as commonly
  misunderstood.
phase: 4
triggers:
  - "grill me"
  - "stress-test me on"
  - "interview me on"
  - learner claims mastery on a sub-topic with an open Medium-or-higher gap
inputs:
  - "topic-config.md: §3 (target sub-topic), §6 (verification level)"
  - "progress/<tracker>: §\"Per-Domain Detail\" for the sub-topic, §\"Knowledge Gaps\""
outputs:
  - "session-notes: appends a \"Grill Session\" block with full Q&A"
  - "tracker: updates §\"Per-Domain Detail\" (may promote `[~]`→`[x]` or demote `[x]`→`[~]`); adds/closes gaps as the grill exposes them"
verification: inherits
---

## Purpose

Adapted from `mattpocock/skills/grill-me`. Where the default
comprehension check is one short question, this skill is a deliberate
multi-turn interview that walks the dependency tree of a concept,
asking one question at a time. It's how the coach distinguishes
"familiarity" from "operational understanding".

## When to use

- Learner says "I think I've got X" — verify before marking mastered.
- A `mock-exam` revealed inconsistency on a topic the learner thought
  they owned.
- High-stakes sub-topic that's commonly misunderstood (look for
  prior gaps in the tracker).

## When NOT to use

- Learner is fragile or just learned the concept 2 minutes ago — give
  acquisition more time first.
- For procedural/quantitative material — use `worked-example`'s "you
  do" round instead.

## Procedure

1. Confirm the target concept and announce: *"Grilling on {{concept}}.
   I'll ask one question at a time and walk every branch. Say 'pass'
   to skip a branch."*
2. Start at the **root**: definition + boundary (what is it, what is it
   *not*).
3. Walk **branches** in dependency order — each question depends on
   the previous answer being correct. For each question:
   - Provide the coach's recommended answer mentally before asking.
   - Ask the question.
   - Score the answer: ✅ correct / 🟡 partial / ❌ wrong.
   - On ❌, stop the branch, mark the precise misconception, and decide:
     stay-and-fix or note-and-continue.
4. Cover at minimum: definition, mechanism, when-to-apply, when-NOT,
   one edge case, one common confusion with a sibling concept.
5. End with a **summary scorecard** by branch, and an explicit verdict:
   *Mastered / In Progress / Not Yet*.
6. **Tracker write-back** — see below.

## Verification policy hook

Under `strict`, every coach correction during the grill must be backed
by a cited authoritative source from `topic-config.md` §4. Under
`standard`, cite for numbers/dates/rules. Under `relaxed`, cite only
when the learner asks.

## Tracker write-back

- **session-notes** — append a `### Grill Session — {{concept}}` block
  with the full ordered Q&A, the per-branch scores, and the verdict.
- **tracker** —
  - **Mastered** verdict → move sub-topic to `[x]` with confidence
    based on how many branches needed prompting.
  - **In Progress** verdict → keep `[~]` and update *what still needs
    work*.
  - **Not Yet** verdict → demote `[x]` back to `[~]` if it had been
    marked mastered, and open a High-severity gap on the failed branch.

## Example

> **Learner:** "Grill me on Roth conversions."
>
> **Coach (skill: `grill-me-on-concept`):** "Branch 1 — definition. In
> one sentence, what is a Roth conversion?"
>
> *(learner answers ✅)*
>
> "Branch 2 — who pays the tax, when, and on what amount?"
>
> *(learner answers 🟡 — fuzzy on the 'on what amount' part)*
>
> "Pause — is the taxable amount the gross conversion or net of basis?
> …"
>
> *(continues; ends with: Mastered on definition + timing, In Progress
> on pro-rata, gap re-opened on basis treatment.)*
