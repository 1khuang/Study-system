# Init & Daily Workflow

This repository is a **topic-agnostic guided-learning system**. The same
structure works for an exam, a language, a programming framework, or any
other study topic. This file explains the two workflows the AI study coach
follows: **init** (one-time setup) and **daily update** (every session).

---

## 1. Init Workflow (one-time, when starting a new topic)

The goal of init is to produce a filled-in `topic-config.md` and a fresh
`progress/<short-code>-study-tracker.md` so all subsequent sessions have
something to update.

### 1.1 What the learner does

1. Clone or fork this repo.
2. Open it in your coding agent of choice (Claude Code, Codex, opencode,
   etc.) — the agent will pick up `CLAUDE.md` / `AGENTS.md` automatically.
3. Start a chat with the coach and say something like:
   **"I want to study X. Initialize the tracker."**

The coach will notice that `topic-config.md` is in template state and walk
you through filling in **Topic Identity**, **Learning Mode**, **Knowledge
Domains** (with weights), **Materials**, **Milestones**, **Verification
Policy**, and **Personalization**. Weight estimates are fine — they can be
refined later.

> Prefer to fill it in yourself first? You can also edit `topic-config.md`
> directly (or `cp topic-config.template.md topic-config.md` if you've
> already overwritten it) and then ask the coach to initialize.

### 1.2 What the coach does

When asked to initialize (or when it notices `topic-config.md` has been
filled in but no matching tracker exists), the coach must:

1. Read `topic-config.md` end-to-end.
2. Confirm the topic name, target date, and domain weights with the learner
   (one short summary, ask if anything is wrong).
3. Generate `progress/<short-code>-study-tracker.md` by filling in
   `progress/STUDY-TRACKER-TEMPLATE.md` with:
   - All domains and sub-topics from the config (every sub-topic starts as
     `[ ]` not-yet-studied).
   - Quick Stats with zeroed counts.
   - Domain Progress table sorted by weight.
   - Empty Knowledge Gaps and Session Index.
   - A first-pass Next-Step Study Plan that prioritizes high-weight,
     zero-coverage domains.
4. Make sure `/sessions/` exists.
5. Tell the learner the tracker is ready and propose the first session topic
   based on the highest-priority domain.

### 1.3 Re-initialization

If `topic-config.md` changes materially (new domains, new weights, new
target date), the coach should:

- Preserve all existing mastered topics and resolved gaps.
- Add any new sub-topics as `[ ]` not-yet-studied.
- Recompute the Domain Progress table and Next-Step Study Plan.
- Note the change at the top of the tracker (one line: "Re-initialized on
  YYYY-MM-DD: <reason>").

---

## 2. Daily Update Workflow (every session)

After **every** learning conversation, the coach must complete two steps in
order. Skipping either step breaks the system.

### Step 1 — Write detailed session notes

Create `/sessions/YYYY-MM-DD/session-notes.md` from
`sessions/SESSION-TEMPLATE.md`. Capture, at minimum:

- Date, duration, main topics
- Each question the learner asked (verbatim when reasonable) and their
  starting understanding
- The explanation given and which materials/sources were cited
- Comprehension checks and how the learner answered them
- **New knowledge gaps** discovered this session
- **Resolved knowledge gaps** closed this session
- **Sub-topics newly mastered** (with confidence level)
- Action items / what to do next session

If a session-notes file already exists for today, **edit** it (do not
overwrite).

### Step 2 — Update the tracker

Update `progress/<short-code>-study-tracker.md`:

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
8. Append a row to the **Session Index** linking today's notes.

### Step 3 — Verify (lightweight)

Before ending the session, the coach should:

- Re-read the tracker's Quick Stats and confirm they match what's in the
  per-domain sections.
- If the topic-config sets verification level `strict`, double-check that
  every factual claim made in the session has a cited source in the notes.

---

## 3. Notes on Repo Defaults

The repo ships as a clean template:

- `topic-config.md` is a copy of `topic-config.template.md` (placeholders
  unfilled). The coach treats this as "init mode".
- `progress/` contains only `STUDY-TRACKER-TEMPLATE.md`. A
  `progress/<short-code>-study-tracker.md` is generated on first init.
- `sessions/` contains only `SESSION-TEMPLATE.md`. Per-day folders
  (`sessions/YYYY-MM-DD/`) are created by the coach as you work.

To switch topics later, just overwrite `topic-config.md` (back it up first
if you want) and ask the coach to re-initialize. Existing trackers under
`progress/` are kept and a new sibling tracker is generated for the new
topic.
