---
name: teach-back
description: >
  Have the learner teach the coach a concept end-to-end (Feynman
  technique / protégé effect), with the coach playing a curious
  novice who asks "why" until either the learner reaches a clean
  explanation or a fuzzy spot is exposed. Use when the learner says
  "let me teach you", "Feynman this", "I want to explain it back", or
  to consolidate a sub-topic before marking mastered.
phase: 8
phases_secondary: [4]
triggers:
  - "let me teach you"
  - "Feynman this"
  - "I want to explain it back"
  - learner is about to be marked mastered on a sub-topic and wants a final check
inputs:
  - "topic-config.md: §3 (sub-topic), §6 (verification)"
  - "progress/<tracker>: §\"Per-Domain Detail\" for the sub-topic"
outputs:
  - "session-notes: appends a \"Teach-Back\" block with the learner's explanation, the coach's questions, and the resulting score"
  - "tracker: may promote sub-topic to `[x]`, may open or close gaps"
verification: inherits
---

## Purpose

Teaching is the strongest test of understanding (the protégé effect).
The default loop has the coach explaining and the learner answering;
this skill flips the roles. The coach plays a friendly, curious
novice — not adversarial like `grill-me-on-concept` — and probes
gently for the first place the explanation gets fuzzy.

## When to use

- Learner is close to mastery and wants a final consolidation pass.
- Cross-domain synthesis (Phase 8): "teach me how Domain B and
  Domain F connect".
- After a long acquisition session, as a self-check.

## When NOT to use

- The learner is still in early acquisition — they don't have enough
  yet to teach. Use `worked-example` instead.
- High-pressure moments (right before a real exam) where a fuzzy
  teach-back will spike anxiety. Use `retrieval-quiz` instead.

## Procedure

1. Confirm the topic and the audience persona: *"I'll play someone
   who knows {{adjacent thing}} but has never heard of {{this}}.
   Teach me from scratch."*
2. Stay quiet while the learner gives an opening explanation.
3. Ask **"why?" or "how?" follow-ups** at the points where the
   explanation got vague, hand-wavy, or used jargon without defining
   it. Aim for 3–5 follow-ups.
4. If the learner uses a metaphor, ask where it breaks down — feeds
   into `analogy-bank`.
5. Ask one **transfer** question: *"How would you re-teach this to
   someone whose only background is {{very different field}}?"* This
   forces the learner to find the structural core.
6. Score: **clean teach** (no fuzzy spots), **fuzzy spots noted**, or
   **broke down** (couldn't recover). Tell the learner the score and
   why.
7. **Tracker write-back** — see below.

## Verification policy hook

The coach must not silently accept incorrect statements just because
the learner is "teaching". Under `strict`, every factual statement
the learner makes that the coach is uncertain about must be checked
against authoritative material before the score is assigned. The
gentle-novice tone applies to questions, not to fact-checking.

## Tracker write-back

- **session-notes** — append a `### Teach-Back — {{sub-topic}}` block
  with the persona, the learner's explanation transcript (or
  summary), the coach's follow-ups, and the score.
- **tracker** —
  - **Clean teach** → promote `[~]` → `[x]` (confidence High); close
    related gaps.
  - **Fuzzy spots** → keep `[~]`; open precise Medium gaps on each
    fuzzy spot.
  - **Broke down** → keep `[~]`, open a High gap, and queue
    `worked-example` or re-acquisition in `Next-Step Study Plan`.

## Example

> **Learner:** "Let me teach you Roth conversions."
>
> **Coach (skill: `teach-back`):** "I know roughly what an IRA is and
> nothing about taxes. Go."
>
> *(learner explains)*
>
> "Wait — you said 'tax-free growth from then on'. Why is it tax-free?
> What changed?"
>
> *(learner clarifies; coach asks 2 more 'why's; one fuzzy spot on
> the 5-year rule)*
>
> "Score: fuzzy spots noted — the 5-year rule. Logging that as a
> Medium gap; everything else holds. Want to do a 5-min mini-lesson
> on the 5-year rule now?"
