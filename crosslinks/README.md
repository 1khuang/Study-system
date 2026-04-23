# `crosslinks/` — Cross-Topic Insights

When studying multiple topics in parallel, the most valuable moments
are often the ones where two topics suddenly **connect** — the same
underlying idea showing up under different names, a technique from one
domain illuminating another, or a contradiction that forces you to
sharpen both.

`crosslinks/` is where those moments are recorded. They are
**first-class artifacts**, not buried inside any single topic's session
notes, so they are easy to revisit and easy to find from either side
of the connection.

---

## When a crosslink gets created

The coach creates a crosslink whenever **either** of these is true:

- The learner says something like _"this is just like X from my Y
  topic"_ or _"isn't this related to what we did in Z?"_, **and** Z is
  a different topic that already exists under `topics/`.
- The learner explicitly asks the coach to compare two topics
  (_"compare AWS IAM with the trust-permissions stuff from CFP"_).
- The coach itself notices a meaningful structural parallel between
  the current question and material from another active topic.

Coach behavior is defined in [`../CLAUDE.md`](../CLAUDE.md) §9.

---

## Layout

```
crosslinks/
  README.md                    ← this file
  INDEX.md                     ← reverse-chronological index of all crosslinks
  CROSSLINK-TEMPLATE.md        ← copy this when creating a new one
  YYYY-MM-DD-<slug>.md         ← one file per insight
```

Filename convention: `YYYY-MM-DD-<short-slug>.md`, e.g.
`2026-04-23-iam-vs-trust-permissions.md`.

---

## File format

Every crosslink starts with YAML front-matter that machine-identifies
the topics and sub-topics it touches, followed by free-form prose:

```markdown
---
date: 2026-04-23
topics: [aws-saa, cfp]
links:
  - aws-saa:A.3   # IAM policies & permissions
  - cfp:F.51      # Trustee discretionary powers
session_refs:
  - topics/aws-saa/sessions/2026-04-23.md
  - topics/cfp/sessions/2026-04-23.md
tags: [permissions, principle-of-least-privilege]
---

## The connection

…1–3 paragraphs explaining what the parallel actually is, why it is
non-obvious, and what it lets you do.

## Implications for each topic

- **aws-saa**: …
- **cfp**: …

## Follow-ups

- [ ] Add a comprehension check to next AWS session that uses the
      trust-style framing.
- [ ] Re-examine CFP F.51 wording in light of "explicit deny wins".
```

---

## Two-way registration

Whenever a crosslink file is created, the coach must also append a row
to the **Cross-topic Links** section of **each** participating topic's
`tracker.md`, with a relative path to the crosslink. This guarantees
that from any topic you can discover all the cross-domain insights it
participates in, and from `INDEX.md` you can survey them globally.

---

## When _not_ to use a crosslink

- A passing analogy used to explain a single concept inside one topic
  → keep it inline in that session's notes.
- Two sub-topics inside the **same** topic that are related → that
  belongs in the topic's own tracker, not in `crosslinks/`.
- A vague feeling of similarity with no actionable implication →
  skip; crosslinks should earn their place.
