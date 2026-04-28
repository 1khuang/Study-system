# `topics/` — One Home Per Study Topic

Each topic the learner is working on lives in its own directory under
`topics/`. This is what makes it safe to study **multiple topics in
parallel** (even on the same day) without files colliding or trackers
getting tangled.

---

## Layout

```
topics/
  <short-code>/                ← one directory per topic
    topic-config.md            ← what this topic is (filled-in copy of
                                 ../../topic-config.template.md)
    tracker.md                 ← progress tracker for this topic
                                 (filled-in copy of
                                 ../../progress/STUDY-TRACKER-TEMPLATE.md)
    sessions/
      YYYY-MM-DD.md            ← one note file per study day, per topic
                                 (filled-in copy of
                                 ../../../sessions/SESSION-TEMPLATE.md)
    notes/
      <domain-code>-<slug>.md  ← one chapter notes file per domain
                                 (filled-in copy of
                                 ../../../notes/NOTES-TEMPLATE.md).
                                 Distilled, organized-by-syllabus
                                 reference of definitive knowledge
                                 the coach has surfaced in
                                 conversation. See ../notes/README.md.
```

`<short-code>` is the same `Short Code` you set in §1 of the topic's
`topic-config.md` (e.g. `aws-saa`, `cfp`, `linalg`, `spanish`).

### Why per-topic directories

If the learner studies **CFP** in the morning and **AWS** in the
afternoon on the same day, there are simply two files:

```
topics/cfp/sessions/2026-04-23.md
topics/aws-saa/sessions/2026-04-23.md
```

Each tracker's Session Index only references its own topic's sessions,
so the timeline stays clean and per-topic mastery is never confused.

---

## The active topic pointer

The repo root contains a `.active-topic` file holding a single line —
the `<short-code>` of the topic the coach should default to in the
current chat:

```
aws-saa
```

The coach reads `.active-topic` at the start of every session to decide
which topic's `topic-config.md`, `tracker.md`, and most recent session
file to load. To switch, just say _"switch to cfp"_ in chat and the
coach will rewrite `.active-topic` (and announce the switch so you can
correct it).

If `.active-topic` is missing or names a non-existent topic and only
**one** topic exists under `topics/`, the coach uses that one. If
multiple topics exist and there is no pointer, the coach asks which one
to use before teaching.

---

## Adding a new topic

See [`../INIT.md`](../INIT.md) §1. In short:

1. Pick a `<short-code>`.
2. `cp topic-config.template.md topics/<short-code>/topic-config.md`
   and fill it in (or ask the coach to walk you through it).
3. The coach generates `topics/<short-code>/tracker.md` from
   `progress/STUDY-TRACKER-TEMPLATE.md`.
4. The coach updates `.active-topic` to `<short-code>` and proposes the
   first session.

---

## Cross-topic insights

Discoveries that span two or more topics ("oh, this AWS IAM idea is
basically the same as that legal-trust concept") are first-class and
live under [`../crosslinks/`](../crosslinks/), not buried inside any
single topic's session notes. Each topic's `tracker.md` has a
**Cross-topic Links** section that back-references the crosslinks it
participates in, so you can navigate from either side.
