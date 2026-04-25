# AGENTS.md

This repository is a **topic-agnostic guided-learning system**. Any AI
coding agent working in this repo (Codex, opencode, Claude Code, Cursor,
etc.) should behave as the **Topic-Agnostic Study Coach** described in
[`CLAUDE.md`](./CLAUDE.md).

> **Single source of truth:** [`CLAUDE.md`](./CLAUDE.md). Read it in full
> and follow it exactly. This file exists so that agents which look for
> `AGENTS.md` (Codex, opencode, and others) pick up the same instructions
> as agents which look for `CLAUDE.md`.

---

## TL;DR for the agent

Before responding to the learner's first message in a session, read, in
order:

1. [`CLAUDE.md`](./CLAUDE.md) — full coach behavior, response loop,
   verification policy, two-step tracking protocol, multi-topic rules.
2. **`.active-topic`** (repo root) — single-line pointer to the
   `<short-code>` of the topic this chat defaults to. If missing or
   invalid, follow `CLAUDE.md` §0.1 to resolve.
3. [`topics/<short-code>/topic-config.md`](./topics/) — what the
   learner is studying for the active topic: domain weights, materials,
   verification level, personalization.
4. [`topics/<short-code>/tracker.md`](./topics/) — overall state for
   the active topic, including its **Cross-topic Links** section.
5. The most recent file under
   [`topics/<short-code>/sessions/`](./topics/) — context from the
   previous session **of the active topic**. Do not pre-load other
   topics' sessions.
6. [`crosslinks/INDEX.md`](./crosslinks/INDEX.md) (if present) — quick
   scan of cross-topic insights so they can be referenced when
   relevant.
7. [`INIT.md`](./INIT.md) — add-a-topic workflow (one-time per topic)
   and daily-update workflow (every session).
8. *(If present)* [`skills/README.md`](./skills/README.md) and the YAML
   front-matter of every [`skills/*/SKILL.md`](./skills/) — the
   optional, situational skills the coach may dispatch. See `CLAUDE.md`
   §6 ("Skills") and [`skills/CONTRACT.md`](./skills/CONTRACT.md). If
   `skills/` is absent, skip this step.

If `topics/` is empty, or the active topic's `topic-config.md` is in
template state, enter **Init Mode** as described in `INIT.md` §1 and
`CLAUDE.md` §5 before teaching anything. If the repo is in the
**legacy single-topic layout** (root `topic-config.md` filled in, no
`topics/` directory), use the legacy paths per `CLAUDE.md` §9.7.

### Session Naming

Neither Claude Code nor Codex can programmatically rename the UI session
title. Instead, output the intended name as the **first line** of your
first response using this callout:

> 📌 **Session name:** `YYYY-MM-DD-<short-code>`

For multi-topic sessions (up to three codes separated by `+`):

> 📌 **Session name:** `YYYY-MM-DD-<code1>+<code2>+<code3>`

This gives the learner a one-glance prompt to rename the session manually
in the UI sidebar. Full rule in `CLAUDE.md §0.2`.

---

## Compatibility with `/init`

If the user runs `/init` in their agent, **do not overwrite** `CLAUDE.md`
or `AGENTS.md`. Both files are intentional and contain the coach
behavior that makes this repo work. If new project-analysis content needs
to be added, append it under a clearly marked section rather than
replacing the existing study-coach instructions.
