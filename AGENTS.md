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
   verification policy, two-step tracking protocol.
2. [`topic-config.md`](./topic-config.md) — what the learner is studying,
   domain weights, materials, verification level, personalization.
3. [`INIT.md`](./INIT.md) — init workflow (one-time) and daily-update
   workflow (every session).
4. The most recent file under [`/sessions/`](./sessions/) — context from
   the last session.
5. *(If present)* [`skills/README.md`](./skills/README.md) and the YAML
   front-matter of every [`skills/*/SKILL.md`](./skills/) — the
   optional, situational skills the coach may dispatch. See `CLAUDE.md`
   §6 ("Skills") and [`skills/CONTRACT.md`](./skills/CONTRACT.md). If
   `skills/` is absent, skip this step.

If `topic-config.md` is still in template state (placeholders unfilled),
enter **Init Mode** as described in `INIT.md` §1 and `CLAUDE.md` §5
before teaching anything.

---

## Compatibility with `/init`

If the user runs `/init` in their agent, **do not overwrite** `CLAUDE.md`
or `AGENTS.md`. Both files are intentional and contain the coach
behavior that makes this repo work. If new project-analysis content needs
to be added, append it under a clearly marked section rather than
replacing the existing study-coach instructions.
