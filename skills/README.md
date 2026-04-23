# Skills Index

> Optional, situational behaviors that the AI study coach may dispatch
> when a learner's intent matches. Skills are **additive** — if this
> directory is removed, the system behaves exactly as the base coach in
> [`../CLAUDE.md`](../CLAUDE.md).
>
> - **Authoring contract:** [`CONTRACT.md`](./CONTRACT.md)
> - **Phase model & audit:** [`../docs/learning-flow.md`](../docs/learning-flow.md)
> - **How dispatch works:** see `CONTRACT.md` §4 and `CLAUDE.md` §6
>   ("Skills (optional, situational)").

---

## Skills by phase

> **A note on the examples inside each `SKILL.md`.** The `## Example`
> sections use concrete dialogue from the repo's origin case study
> (CFP-exam prep) because concrete examples land better than abstract
> ones. The skill's `## Purpose`, `## When to use`, `## Procedure`,
> and `## Tracker write-back` sections are deliberately
> topic-agnostic — those are the parts the coach actually executes.
> See `CONTRACT.md` §3 rule 4 for the exact policy.

| Phase | Skill | Trigger words / conditions |
|------:|-------|----------------------------|
| 1. Onboarding & diagnosis | [`diagnose-baseline`](./diagnose-baseline/SKILL.md) | "what do I already know about", "diagnose me on", new sub-topic |
| 2. Planning | [`replan-priorities`](./replan-priorities/SKILL.md) | "re-prioritize", "what should I focus on", target date changed |
| 2 / 7. Planning + reflection | [`schedule-spaced-reviews`](./schedule-spaced-reviews/SKILL.md) | "schedule reviews", "what should I review today", new mastered item |
| 3. Acquisition | [`worked-example`](./worked-example/SKILL.md) | "show me how", "walk me through an example" |
| 3. Acquisition | [`analogy-bank`](./analogy-bank/SKILL.md) | "give me an analogy", "try another way", failed comprehension check |
| 4. Active processing | [`grill-me-on-concept`](./grill-me-on-concept/SKILL.md) | "grill me", "stress-test me on" |
| 4 / 8. Active processing + transfer | [`concept-map`](./concept-map/SKILL.md) | "concept map", "how do these connect", before a mock exam |
| 5. Practice & retrieval | [`retrieval-quiz`](./retrieval-quiz/SKILL.md) | "quiz me", "test me on", invoked by spaced reviews |
| 5. Practice & retrieval | [`mixed-practice`](./mixed-practice/SKILL.md) | "mixed practice", "interleave", 3+ in-progress sub-topics |
| 6. Assessment & feedback | [`mock-exam`](./mock-exam/SKILL.md) | "mock exam", "practice test", milestone |
| 6. Assessment & feedback | [`error-postmortem`](./error-postmortem/SKILL.md) | "post-mortem this", "why did I get this wrong", auto from `mock-exam` |
| 7. Reflection & metacognition | [`confidence-calibration`](./confidence-calibration/SKILL.md) | "calibrate me", "am I overconfident", every ~20 items |
| 8. Consolidation & transfer | [`teach-back`](./teach-back/SKILL.md) | "let me teach you", "Feynman this" |

## Learner-facing cheat sheet

Things you can say to invoke a skill explicitly:

- **"Diagnose me on {{topic}}"** → `diagnose-baseline`
- **"What should I focus on?" / "Re-prioritize"** → `replan-priorities`
- **"What should I review today?"** → `schedule-spaced-reviews`
- **"Show me how / walk me through an example"** → `worked-example`
- **"Give me an analogy / try another way"** → `analogy-bank`
- **"Grill me on {{concept}}"** → `grill-me-on-concept`
- **"Draw a concept map / how do these connect?"** → `concept-map`
- **"Quiz me on {{scope}}"** → `retrieval-quiz`
- **"Mixed practice / interleave me"** → `mixed-practice`
- **"Mock exam / practice test"** → `mock-exam`
- **"Post-mortem this miss"** → `error-postmortem`
- **"Calibrate me / am I overconfident?"** → `confidence-calibration`
- **"Let me teach you {{concept}} / Feynman this"** → `teach-back`

If you don't say any of these, the coach stays in its default Socratic
loop from `CLAUDE.md` §1.

## Per-topic opt-in / opt-out

Each topic can disable specific skills via `topic-config.md` §8
("Enabled Skills"). For example, a language-acquisition topic typically
disables `mock-exam`. See [`../topic-config.template.md`](../topic-config.template.md)
for the format.

## Adding a new skill

Follow the checklist in [`CONTRACT.md`](./CONTRACT.md) §5 and add a
row to the table above.
