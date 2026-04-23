# Learning Flow & Skills Audit

> Reference model for **how** the Study-System teaches, independent of any
> specific topic. This document is the canvas the `skills/` directory is
> built against. If a proposed skill doesn't fit a phase below — or it
> duplicates something already in `CLAUDE.md` — it should not be added.

This file is the deliverable for **Step 0 (audit)** and **Step 1 (decompose
the flow)** of the skill-integration plan. Steps 2–5 (research, contract,
integration, pilot) live in `skills/CONTRACT.md`, `skills/README.md`, and
the per-skill `SKILL.md` files.

---

## Part A — Audit: what the system already does

The coach behavior in `CLAUDE.md` / `AGENTS.md` / `INIT.md` already covers
a lot. Skills must **fill gaps**, not duplicate the loop.

### Already covered (coach-core — stays in `CLAUDE.md`, do **not** turn into a skill)

| Behavior | Where it's defined |
|---|---|
| Read `topic-config.md` + tracker + last session at session start | `CLAUDE.md` §0, `AGENTS.md` TL;DR |
| Socratic response loop: probe baseline → ~200-word explanation → comprehension check → adapt | `CLAUDE.md` §1 |
| Adaptive follow-up (different angle on miss; advance on hit) | `CLAUDE.md` §1 |
| Verification policy: `strict` / `standard` / `relaxed` citation rules | `CLAUDE.md` §4 |
| Two-step tracking protocol: write `session-notes.md`, then update the tracker | `CLAUDE.md` §3, `INIT.md` §2 |
| Single-source-of-truth rule: no ad-hoc bookkeeping files | `CLAUDE.md` §3 ("Hard rules") |
| Init / re-init flow | `INIT.md` §1, `CLAUDE.md` §5 |
| Re-prioritization formula `weight × (1 − coverage) + open_gap_boost` | `INIT.md` §2 step 7 |
| Tone / Do-Don't | `CLAUDE.md` §1 |

### Partially covered (a skill can sharpen, but must defer to coach-core)

| Behavior | Status | Skill opportunity |
|---|---|---|
| Diagnostic interview at the start of a brand-new sub-topic | Implicit in §1 step 1 ("what do you already know?") | A `diagnose-baseline` skill formalizes it as a procedure with tracker output |
| Prioritization / weekly re-plan | Done automatically each session | A `replan-priorities` skill lets the learner *explicitly* request a deeper re-plan (e.g. after target-date change) |
| Comprehension check | Always one short check after each explanation | A `grill-me-on-concept` skill provides a multi-turn, branch-resolving stress-test on demand |

### Not covered today (genuine gaps — these are where skills earn their place)

1. **Spaced-repetition scheduling** (Ebbinghaus / SM-2 style review queue).
2. **Retrieval practice** (active recall sessions distinct from the per-explanation comprehension check).
3. **Interleaved / mixed practice** across multiple sub-topics in one sitting.
4. **Mock exams / timed assessments** with per-question diagnosis.
5. **Error post-mortem** — structured analysis of *why* a wrong answer was wrong.
6. **Concept maps** showing how sub-topics connect within and across domains.
7. **Worked-example walkthroughs** with explicit "I do / we do / you do" scaffolding.
8. **Analogy banks** that build a per-learner library of analogies that have worked.
9. **Confidence calibration** — comparing self-rated confidence vs actual correctness over time.
10. **Teach-back / Feynman exercises** that force synthesis and surface fuzzy understanding.

The skills in `skills/` map 1:1 to gaps 1–10 (plus the three "sharpening"
candidates above), giving ~13 skills total.

---

## Part B — The reference learning flow (8 phases)

Each phase lists its **purpose**, the **classical pedagogy** it draws on,
the **AI delta** (what changes when the teacher is an always-available
assistant), and the **skills** mapped to it. Skill names link to files in
`skills/`.

### Phase 1 — Onboarding & diagnosis
- **Purpose:** establish the goal, surface prior knowledge, calibrate
  domain weights and starting coverage.
- **Pedagogy:** Vygotsky's zone of proximal development; pre-assessment;
  goal-setting theory.
- **AI delta:** the coach can *interview* the learner and auto-generate
  the tracker (`INIT.md`), removing weeks of curriculum-design overhead.
- **Coach-core:** init mode in `CLAUDE.md` §5 / `INIT.md` §1.
- **Skills:** [`diagnose-baseline`](../skills/diagnose-baseline/SKILL.md).

### Phase 2 — Planning
- **Purpose:** decide what to study next, when, and in what order.
- **Pedagogy:** backward design (Wiggins & McTighe); curriculum
  sequencing; Pareto prioritization.
- **AI delta:** the plan is *recomputed every session* from a tiny
  formula, instead of being set once and growing stale.
- **Coach-core:** automatic recompute after every session
  (`INIT.md` §2 step 7).
- **Skills:** [`replan-priorities`](../skills/replan-priorities/SKILL.md),
  [`schedule-spaced-reviews`](../skills/schedule-spaced-reviews/SKILL.md).

### Phase 3 — Acquisition
- **Purpose:** present new material clearly and connect it to what the
  learner already knows.
- **Pedagogy:** direct instruction; I-do / we-do / you-do; Mayer's
  multimedia / dual-coding principles.
- **AI delta:** unlimited re-explanation at any granularity, with
  examples customized to the learner's context.
- **Coach-core:** ~200-word explanation step in `CLAUDE.md` §1.
- **Skills:** [`worked-example`](../skills/worked-example/SKILL.md),
  [`analogy-bank`](../skills/analogy-bank/SKILL.md).

### Phase 4 — Active processing
- **Purpose:** force the learner to *do something* with new material —
  rephrase it, defend it, connect it, apply it — so it sticks.
- **Pedagogy:** generative learning; elaborative interrogation;
  Feynman technique.
- **AI delta:** an infinitely patient interlocutor that never gets tired
  of the next "but why?".
- **Coach-core:** comprehension check in `CLAUDE.md` §1.
- **Skills:** [`grill-me-on-concept`](../skills/grill-me-on-concept/SKILL.md),
  [`concept-map`](../skills/concept-map/SKILL.md).

### Phase 5 — Practice & retrieval
- **Purpose:** strengthen memory traces by pulling information *out*
  rather than putting it in.
- **Pedagogy:** retrieval practice (Roediger & Karpicke); interleaving;
  desirable difficulties (Bjork).
- **AI delta:** infinite custom items; difficulty adapts in real time.
- **Coach-core:** none — this is a real gap today.
- **Skills:** [`retrieval-quiz`](../skills/retrieval-quiz/SKILL.md),
  [`mixed-practice`](../skills/mixed-practice/SKILL.md).

### Phase 6 — Assessment & feedback
- **Purpose:** simulate the real performance condition (exam, interview,
  conversation) and diagnose errors precisely.
- **Pedagogy:** formative assessment; error analysis; Hattie's
  high-effect-size feedback.
- **AI delta:** every wrong answer can be turned into a personalized
  micro-lesson and a tracked gap.
- **Coach-core:** none for full-form assessment.
- **Skills:** [`mock-exam`](../skills/mock-exam/SKILL.md),
  [`error-postmortem`](../skills/error-postmortem/SKILL.md).

### Phase 7 — Reflection & metacognition
- **Purpose:** learner builds an accurate model of *what they know vs
  think they know*, and schedules review accordingly.
- **Pedagogy:** metacognition; self-explanation; spaced repetition
  (Ebbinghaus, SM-2).
- **AI delta:** automatic gap journaling and surfacing across sessions.
- **Coach-core:** gap tables + session index in tracker.
- **Skills:** [`confidence-calibration`](../skills/confidence-calibration/SKILL.md)
  (and [`schedule-spaced-reviews`](../skills/schedule-spaced-reviews/SKILL.md)
  re-applied here — many skills span two phases).

### Phase 8 — Consolidation & transfer
- **Purpose:** synthesize across domains; apply knowledge to novel
  problems and real-world projects.
- **Pedagogy:** far transfer; project-based learning; protégé effect
  ("learning by teaching").
- **AI delta:** the coach can play the role of student / interviewer /
  client, on demand.
- **Coach-core:** none for explicit synthesis exercises.
- **Skills:** [`teach-back`](../skills/teach-back/SKILL.md),
  [`concept-map`](../skills/concept-map/SKILL.md) (cross-domain mode).

---

## Part C — Coach-core vs skill: the rule

| If the behavior is… | Then it lives in… |
|---|---|
| Required on **every turn** of **every session** (Socratic loop, citation policy, tracker write-back, init flow) | `CLAUDE.md` / `AGENTS.md` / `INIT.md` |
| **Situational** — useful only when the learner asks for it or a specific condition is met (quiz me, mock exam, schedule reviews, post-mortem this wrong answer) | A `skills/<name>/SKILL.md` |

A skill is **never** allowed to:
- Replace the Socratic loop.
- Bypass the verification policy.
- Create a new bookkeeping file outside `progress/<short-code>-study-tracker.md`
  and `sessions/YYYY-MM-DD/session-notes.md`.

---

## Part D — How to extend this document

When adding a new skill:

1. Confirm the gap it fills isn't already in Part A's "Already covered".
2. Place it in the right phase in Part B (or argue for a new phase).
3. Write the SKILL.md against the contract in
   [`skills/CONTRACT.md`](../skills/CONTRACT.md).
4. Register it in [`skills/README.md`](../skills/README.md).
5. (Optional) list it under `## 8. Enabled Skills` in
   `topic-config.template.md` so per-topic opt-in is documented.
