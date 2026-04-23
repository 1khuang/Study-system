# Topic Configuration

> This file defines **what** the AI study coach is helping you learn.
> The coach reads this file at the start of every session and uses it to
> generate the progress tracker, prioritize topics, and tailor explanations.
>
> Copy this file to `topics/<short-code>/topic-config.md` and fill it in.
> (The `<short-code>` is the slug you set in §1 below.) You can re-init at
> any time by editing this file — the coach will adapt.
>
> A legacy single-topic layout where `topic-config.md` lives in the repo
> root is still recognized for backwards compatibility; see
> [`INIT.md`](./INIT.md) §3.

---

## 1. Topic Identity

- **Topic Name**: <!-- e.g. "AWS Solutions Architect Associate", "Conversational Spanish", "Linear Algebra" -->
- **Short Code**: <!-- short slug used in tracker filenames, e.g. `aws-saa`, `spanish`, `linalg` -->
- **Learner**: <!-- your name or handle (optional) -->
- **Started On**: <!-- YYYY-MM-DD -->
- **Target Date**: <!-- YYYY-MM-DD if you have an exam / deadline; otherwise "Ongoing" -->
- **Goal Statement**: <!-- 1-2 sentences. e.g. "Pass the AWS SAA-C03 exam on first attempt with >80% score" -->

## 2. Learning Mode

- **Mode**: <!-- one of: `exam-prep` | `skill-building` | `concept-mastery` | `language-acquisition` | `project-driven` -->
- **Session Cadence**: <!-- e.g. "5 sessions/week, ~45 min each" -->
- **Preferred Style**: <!-- e.g. "Socratic dialogue + worked examples + spaced retrieval quizzes" -->

## 3. Knowledge Domains

> List the major areas of the topic. Each domain has a **weight** (relative
> importance, in %), an optional **target topic count**, and a list of
> sub-topics. The coach uses weights to prioritize what to study next.
>
> Weights should sum to ~100. If you don't know the official weighting,
> estimate it — you can refine later.

| Code | Domain | Weight | Sub-topic Count |
|------|--------|-------:|----------------:|
| A    | <!-- Domain name -->        | <!-- % --> | <!-- N --> |
| B    | <!-- Domain name -->        | <!-- % --> | <!-- N --> |
| C    | <!-- Domain name -->        | <!-- % --> | <!-- N --> |

### Domain A: <!-- name -->
- A.1 <!-- sub-topic -->
- A.2 <!-- sub-topic -->

### Domain B: <!-- name -->
- B.1 <!-- sub-topic -->
- B.2 <!-- sub-topic -->

### Domain C: <!-- name -->
- C.1 <!-- sub-topic -->
- C.2 <!-- sub-topic -->

## 4. Study Materials

> Anything the coach should know about (and prefer to cite) when teaching.
> Materials are **not** required to be in the repo — they can be external.

| Material | Type | Location | Notes |
|----------|------|----------|-------|
| <!-- e.g. "Official Study Guide" --> | book/pdf/video/site | <!-- path or URL --> | <!-- chapters that matter --> |

## 5. Milestones (optional)

> Optional checkpoints between today and the target date.
> The coach will surface these in the tracker and weekly reviews.

- [ ] <!-- YYYY-MM-DD: Finish first pass of Domain A -->
- [ ] <!-- YYYY-MM-DD: First full-length practice test -->
- [ ] <!-- YYYY-MM-DD: All domains at >70% mastery -->

## 6. Verification Policy

> How strict should the coach be about citing sources?
> Pick the level appropriate for your topic.

- **Level**: <!-- one of: `strict` | `standard` | `relaxed` -->
  - `strict`  — always search & cite authoritative sources before answering
              (use for: certifications, medicine, law, tax/finance, anything
              with annually-changing facts).
  - `standard` — cite sources for specific facts, formulas, dates, statistics.
  - `relaxed`  — explanation-first, cite only when the learner asks
              (use for: foundational concepts, language, intro coursework).
- **Authoritative Sources**: <!-- bullet list, e.g. official docs sites, regulator pages -->

## 7. Personalization

- **Prior Knowledge**: <!-- what you already know coming in -->
- **Known Weak Areas**: <!-- topics you struggle with or want extra time on -->
- **Learning Preferences**: <!-- e.g. "lots of analogies", "show me the math", "always give a worked example" -->

## 8. Enabled Skills (optional)

> Skills are **opt-in, situational** behaviors defined in `skills/`.
> The coach dispatches them when the learner's intent matches a skill's
> trigger. See [`skills/README.md`](./skills/README.md) for the full
> list and [`skills/CONTRACT.md`](./skills/CONTRACT.md) for how dispatch
> works.
>
> By default, **all skills under `skills/` are enabled**. Use the
> `disabled:` list below to opt specific skills out for this topic.
> Examples:
> - A language-acquisition topic typically disables `mock-exam`.
> - A loose concept-mastery topic may disable `confidence-calibration`.
> - A short ad-hoc topic may disable `schedule-spaced-reviews`.

```yaml
skills:
  disabled:
    # - mock-exam
    # - schedule-spaced-reviews
  # Optional: a friendlier label or extra notes per enabled skill
  notes:
    # retrieval-quiz: prefer 5 items per pass
```

If you remove the entire `skills/` directory, this section becomes a
no-op and the coach behaves as the base system.

---

_Once this file is filled in, ask the coach to **"initialize the tracker"** and it will generate `topics/<short-code>/tracker.md` from this config and point `.active-topic` at this topic._
