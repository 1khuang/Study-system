---
name: concept-map
description: >
  Build a small, learner-drawn map of how concepts in the current topic
  connect — within a single sub-topic, across a domain, or across
  multiple domains for synthesis. Use when the learner says "draw a
  concept map", "how do these connect", "I'm losing the big picture",
  or before a mock exam to consolidate.
phase: 4
phases_secondary: [8]
triggers:
  - "concept map"
  - "how do these connect"
  - "I'm losing the big picture"
  - "synthesize across domains"
  - prepping for a mock-exam
inputs:
  - "topic-config.md: §3 (domains and sub-topics)"
  - "progress/<tracker>: §\"Per-Domain Detail\" (mastered + in-progress sub-topics), §\"Concept Maps\" (created on first use)"
outputs:
  - "tracker: appends to §\"Concept Maps\""
  - "session-notes: appends a \"Concept Map\" block including the map (Mermaid or plain indented list) and the new connections found"
verification: inherits
---

## Purpose

Concepts decay in isolation. A concept map forces the learner to make
the relationships between sub-topics explicit (cause → effect, contains,
contrasts-with, is-special-case-of). The coach co-builds it; the
learner does the labeling. Output is stored in the tracker so future
sessions can reference and extend the same map.

## When to use

- Learner reports "the pieces don't fit together".
- After mastering 3+ sub-topics in one domain — time to consolidate.
- Before a `mock-exam` to surface cross-cutting links.
- Cross-domain synthesis (Phase 8).

## When NOT to use

- Learner is still acquiring the very first sub-topic in a domain —
  there's nothing to map yet.
- Topic is fundamentally non-relational (e.g. a vocabulary list).

## Procedure

1. Pick a **scope**: one sub-topic, one domain, or N specific
   sub-topics across domains.
2. List the nodes (sub-topics or concepts) the map will include — pull
   from the tracker's `Per-Domain Detail`.
3. Ask the learner, one pair at a time, *"How does {{A}} relate to
   {{B}}?"* Allowed edge labels: `causes`, `requires`, `contains`,
   `contrasts-with`, `is-a-special-case-of`, `triggers`,
   `is-measured-by`, or a custom verb if they propose one.
4. Render the map as a Mermaid diagram (or, if Mermaid is too heavy,
   an indented bullet list with edge labels). Coach proposes the
   layout; learner approves or edits.
5. Identify the **3 weakest edges** (ones the learner couldn't label
   confidently). Each becomes a gap.
6. **Tracker write-back** — see below.

## Verification policy hook

Edges represent the learner's understanding, not authoritative claims,
so verification level mostly doesn't bind. Under `strict`, however, if
the learner asserts an edge that contradicts an authoritative source,
the coach must flag and cite the contradiction.

## Tracker write-back

- **tracker** — maintain `## Concept Maps`, with one sub-section per
  saved map: scope, date, the Mermaid/list, and a list of weak edges
  with their target review date.
- **session-notes** — append a `### Concept Map — {{scope}}` block
  with the map and the 3 weak-edge gaps.
- For each weak edge, add or update a row in `## Knowledge Gaps`
  (severity Medium by default; High if the edge crosses two
  high-weight domains).

## Example

> **Learner:** "I keep mixing up traditional vs Roth tax flows. Map it."
>
> **Coach (skill: `concept-map`):**
>
> ```mermaid
> graph LR
>   Contribution-->|pre-tax| TraditionalIRA
>   Contribution-->|after-tax| RothIRA
>   TraditionalIRA-->|taxable on withdrawal| Withdrawal
>   RothIRA-->|tax-free if qualified| Withdrawal
>   TraditionalIRA-->|requires RMDs at 73| RMD
>   RothIRA-.->|no owner RMDs| RMD
> ```
>
> "Weak edge from your earlier answers: the dotted *no-RMDs* line for
> Roth IRAs (you said 'after age 73 same as traditional'). Logging
> that as a Medium gap and queuing it for review."
