# Init & Daily Workflow

This repository is a **topic-agnostic guided-learning system**. The same
structure works for an exam, a language, a programming framework, or any
other study topic — and it can host **many such topics in parallel**,
each with its own home under `topics/<short-code>/`. This file explains
the two workflows the AI study coach follows: **add a topic** (one-time
per topic) and **daily update** (every session).

---

## 1. Add-a-Topic Workflow (one-time, per topic)

The goal is to produce a filled-in `topics/<short-code>/topic-config.md`
and a fresh `topics/<short-code>/tracker.md` so all subsequent sessions
on this topic have something to update.

### 1.1 What the learner does

1. Clone or fork this repo (only once — the same repo can hold many
   topics).
2. Open it in your coding agent of choice (Claude Code, Codex, opencode,
   etc.) — the agent will pick up `CLAUDE.md` / `AGENTS.md` automatically.
3. Start a chat with the coach and say something like:
   **"I want to study X. Add it as a new topic."**
   (Equivalent phrasings: "initialize the tracker", "start a new topic".)

The coach will pick a `<short-code>` slug (or ask you for one), create
`topics/<short-code>/`, and walk you through filling in **Topic
Identity**, **Learning Mode**, **Knowledge Domains** (with weights),
**Materials**, **Milestones**, **Verification Policy**, and
**Personalization**. Weight estimates are fine — they can be refined
later.

> Prefer to fill it in yourself first? You can also
> `cp topic-config.template.md topics/<short-code>/topic-config.md`,
> edit it, and then ask the coach to initialize.

### 1.2 What the coach does

When asked to add a topic (or when it notices a
`topics/<short-code>/topic-config.md` has been filled in but no matching
tracker exists), the coach must:

1. Read `topics/<short-code>/topic-config.md` end-to-end.
2. Confirm the topic name, target date, and domain weights with the learner
   (one short summary, ask if anything is wrong).
3. Generate `topics/<short-code>/tracker.md` by filling in
   `progress/STUDY-TRACKER-TEMPLATE.md` with:
   - All domains and sub-topics from the config (every sub-topic starts as
     `[ ]` not-yet-studied).
   - Quick Stats with zeroed counts.
   - Domain Progress table sorted by weight.
   - Empty Knowledge Gaps and Session Index.
   - Empty **Cross-topic Links** section.
   - A first-pass Next-Step Study Plan that prioritizes high-weight,
     zero-coverage domains.
4. Make sure `topics/<short-code>/sessions/` exists.
5. Set `.active-topic` (in the repo root) to the new `<short-code>` so
   subsequent chats default to this topic.
6. Tell the learner the tracker is ready and propose the first session
   topic based on the highest-priority domain.

### 1.3 Re-initialization

If a topic's `topic-config.md` changes materially (new domains, new
weights, new target date), the coach should:

- Preserve all existing mastered sub-topics and resolved gaps.
- Add any new sub-topics as `[ ]` not-yet-studied.
- Recompute the Domain Progress table and Next-Step Study Plan.
- Note the change at the top of the tracker (one line: "Re-initialized on
  YYYY-MM-DD: <reason>").

### 1.4 Switching the active topic

Adding a second (or third, fourth…) topic works exactly like §1.1–1.2 —
nothing about existing topics is touched. To change which topic the
coach defaults to in a chat, just say _"switch to <short-code>"_ and
the coach will rewrite `.active-topic` and announce the switch.

---

## 2. Daily Update Workflow (every session)

After **every** learning conversation, the coach must complete two steps in
order, **for the active topic**. Skipping either step breaks the system.

### Step 1 — Write detailed session notes

Create `topics/<short-code>/sessions/YYYY-MM-DD.md` from
`sessions/SESSION-TEMPLATE.md` (where `<short-code>` is the active
topic). Capture, at minimum:

- Date, duration, main topics
- Each question the learner asked (verbatim when reasonable) and their
  starting understanding
- The explanation given and which materials/sources were cited
- Comprehension checks and how the learner answered them
- **New knowledge gaps** discovered this session
- **Resolved knowledge gaps** closed this session
- **Sub-topics newly mastered** (with confidence level)
- Action items / what to do next session

If a session-notes file already exists for today **for this topic**,
**edit** it (do not overwrite). Notes for other topics on the same day
live in their own `topics/<other>/sessions/YYYY-MM-DD.md` — never mix.

### Step 2 — Update the tracker

Update `topics/<short-code>/tracker.md`:

1. Bump **Last Updated** and **Days Remaining**.
2. Update Quick Stats counts.
3. For each newly mastered sub-topic: move it to the domain's **✅ Mastered**
   list with date, confidence, and key points.
4. For each in-progress sub-topic: add or update it under **🟡 In Progress**.
5. Update the **Knowledge Gaps** tables: add new gaps at the right severity,
   bump "Sessions Touched" on existing ones, move closed gaps to **Recently
   Resolved**.
6. Recompute the **Domain Progress Summary** counts and statuses.
7. Recompute the **Next-Step Study Plan** using:
   `priority ≈ weight × (1 − coverage) + open_gap_boost`.
   The first item is what the learner should do in the next session.
8. Append a row to the **Session Index** linking today's notes (relative
   path is just `./sessions/YYYY-MM-DD.md`).
9. If a crosslink was created or referenced (see §2a), append a row to
   the **Cross-topic Links** section as well.

### Step 2a — Cross-topic insights (when triggered)

If the session produced a meaningful cross-topic insight (see
`CLAUDE.md` §9.3 for triggers), the coach must additionally:

1. Create `crosslinks/YYYY-MM-DD-<slug>.md` from
   `crosslinks/CROSSLINK-TEMPLATE.md`, with all participating topics
   listed in the front-matter `topics:` array and specific sub-topic
   codes pinned in `links:`.
2. Prepend a row to `crosslinks/INDEX.md` so it stays
   reverse-chronological.
3. **For every participating topic** (not just the active one),
   append a back-reference row to that topic's tracker's
   **Cross-topic Links** section. The coach may touch non-active
   trackers **only** to add this row; no other state may be modified.

### Step 3 — Multi-topic sessions

If the learner studied two (or more) topics in the same chat (after
explicitly switching with _"switch to <short-code>"_), Steps 1 and 2
are repeated **independently per topic visited**. Each topic gets its
own `YYYY-MM-DD.md` session note and its own tracker update. The two
sessions are linked only if a crosslink was created via Step 2a.

### Step 4 — Verify (lightweight)

Before ending the session, the coach should:

- Re-read each updated tracker's Quick Stats and confirm they match
  what's in the per-domain sections.
- If a topic's `topic-config.md` sets verification level `strict`,
  double-check that every factual claim made for that topic in this
  session has a cited source in the notes.

---

## 3. Backwards Compatibility (legacy single-topic layout)

Earlier versions of this repo used a single-topic layout:

```
topic-config.md                        ← in repo root
progress/<short-code>-study-tracker.md
sessions/YYYY-MM-DD/session-notes.md
```

This still works. The coach detects the legacy layout when:

- `topics/` is missing or empty, **and**
- root `topic-config.md` has been filled in (placeholders replaced).

In that case, the coach uses the legacy paths and treats the multi-topic
and crosslink machinery as no-ops. On the **first** response of a
legacy-mode session, the coach should mention once that the learner can
migrate by:

1. `mkdir -p topics/<short-code>/sessions`
2. `git mv topic-config.md topics/<short-code>/topic-config.md`
3. `git mv progress/<short-code>-study-tracker.md topics/<short-code>/tracker.md`
4. `git mv sessions/YYYY-MM-DD topics/<short-code>/sessions/` for each
   day, then renaming each `session-notes.md` to its day file
   (`mv session-notes.md ../YYYY-MM-DD.md`).
5. `echo <short-code> > .active-topic`
6. Update the tracker's Session Index entries to the new
   `./sessions/YYYY-MM-DD.md` paths.

The coach must **not** perform this migration without explicit
consent.

---

## 4. Notes on Repo Defaults

The repo ships as a clean template:

- `topics/` is empty (only `topics/README.md` explains the layout). The
  coach treats this as "no topics yet — enter Init Mode for the first
  one".
- `progress/` contains only `STUDY-TRACKER-TEMPLATE.md` (the source
  template each new topic's `tracker.md` is generated from).
- `sessions/` contains only `SESSION-TEMPLATE.md` (the source template
  each topic's per-day session note is generated from).
- `crosslinks/` contains only `README.md`, `INDEX.md`, and
  `CROSSLINK-TEMPLATE.md`. Real crosslink files appear as the learner
  produces them.
- `.active-topic` does not yet exist; it is created on the first init.
- A blank `topic-config.md` in the repo root is preserved as a
  legacy-compatibility placeholder; it is **not** the file the coach
  reads once `topics/` is populated.

To add another topic later, just ask the coach: _"Add a new topic: Y."_
Each topic's directory is independent; you can have as many as you
like.
