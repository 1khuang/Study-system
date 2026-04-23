# Skill Contract

> Every file under `skills/<skill-name>/SKILL.md` **must** follow this
> contract. The contract exists so that any AI agent (Claude Code, Codex,
> opencode, Cursor, …) can discover, dispatch, and execute skills in a
> uniform way, and so that skill outputs always flow back into the
> existing tracker rather than into parallel bookkeeping files.

This contract is intentionally minimal. It mirrors the
[`mattpocock/skills`](https://github.com/mattpocock/skills) format
(`name` + `description` + body) so external agents recognize our skills,
and adds a small set of repo-specific fields needed to make skills
*hook into* the Study-System.

---

## 1. File layout

```
skills/
  CONTRACT.md             ← this file
  README.md               ← index of all skills, grouped by phase
  <skill-name>/
    SKILL.md              ← the skill itself
    [resources/]          ← optional bundled prompts, templates, examples
```

- `<skill-name>` is `kebab-case`, matches the `name` field exactly.
- One skill per directory. No nested skills.

---

## 2. Required structure of `SKILL.md`

A skill file has two parts: **YAML front-matter** (machine-readable
metadata) and **Markdown body** (the procedure the agent follows).

### 2.1 YAML front-matter

```yaml
---
name: <kebab-case-name>            # required, must match the directory name
description: >                     # required, single sentence; this is the trigger condition
  <what the skill does + when to use it +
   any explicit phrases the learner might say>
phase: <phase number 1–8>          # required, from docs/learning-flow.md
phases_secondary: [<n>, <n>]       # optional, if the skill spans phases
triggers:                          # required, list of plain-English match conditions
  - "<user phrase or coach-detected condition>"
inputs:                            # required, what the skill reads
  - topic-config.md: <which sections>
  - progress/<tracker>: <which sections>
  - sessions/<latest>: <when applicable>
outputs:                           # required, what the skill writes back
  - tracker: <which sections it updates>
  - session-notes: <which sections it appends to>
verification: <inherits | strict | standard | relaxed>   # default: inherits
                                                         # ("inherits" = use the level
                                                         #  set in topic-config.md)
---
```

Field rules:

- `description` is the **dispatch signal**. The coach matches the user's
  intent against every skill's `description` (same convention as
  `mattpocock/skills`). Write it as: *"<verb-phrase>. Use when
  <condition / explicit phrases>."*
- `triggers` lists concrete user phrases (e.g. `"quiz me"`,
  `"mock exam"`) **and** coach-detected conditions (e.g. `"learner just
  got two practice questions wrong in a row"`). The coach may invoke the
  skill on either kind of trigger.
- `inputs` and `outputs` make the data flow explicit. If a skill needs
  to read or write something not listed here, it doesn't comply with the
  contract — extend the front-matter or split the skill.
- `verification: inherits` is the default and almost always correct. Set
  a stricter level only if the skill makes claims that demand it
  regardless of topic (rare).

### 2.2 Markdown body — required sections, in this order

```markdown
## Purpose
One paragraph. What problem this skill solves and why it exists as a
skill instead of being part of the default coach loop.

## When to use
A few bullets. Concrete user requests, plus coach-detected situations
that should trigger this skill.

## When NOT to use
A few bullets. Cases where the default Socratic loop, or a different
skill, is the better choice. This prevents over-firing.

## Procedure
Numbered steps the agent executes. Be specific. The agent should be
able to follow this without further prompting.

## Verification policy hook
One short paragraph. How this skill behaves under each of `strict`,
`standard`, `relaxed`. If the skill cites no facts, say so explicitly.

## Tracker write-back
The exact updates this skill makes to:
- `sessions/YYYY-MM-DD/session-notes.md` (which sections it appends to)
- `progress/<short-code>-study-tracker.md` (mastered / gaps / next-step
  plan / spaced-review queue)
This is the part that prevents skills from becoming a parallel system.

## Example
A short, realistic dialogue or transcript showing the skill in action.
```

Optional sections (add only if they meaningfully help):

- `## Variants` — small named modes (e.g. `mock-exam` has `quick` and
  `full` variants).
- `## Resources` — pointers into `skills/<skill-name>/resources/`.
- `## See also` — related skills.

---

## 3. Hard rules every skill must obey

These are non-negotiable — they preserve the system's invariants.

1. **Single source of truth.** A skill must not create new files outside
   of `sessions/YYYY-MM-DD/session-notes.md` and
   `progress/<short-code>-study-tracker.md`. Optional bundled prompts
   under `skills/<name>/resources/` are fine *as inputs*, but never as
   running state.
2. **Coach-core wins.** Verification policy, Socratic tone, and the
   two-step tracking protocol from `CLAUDE.md` always apply. A skill may
   sharpen them but never override them.
3. **Backward compatible.** If `skills/` is absent or empty, the system
   must behave exactly as before. Skills are additive.
4. **Topic-agnostic.** Skill prose must not assume a specific topic. Use
   `{{topic}}`, `{{domain}}`, `{{sub-topic}}` placeholders, and read
   actual values from `topic-config.md` at runtime. **Exception:** the
   `## Example` section *may* use one concrete topic as a worked
   illustration — concrete examples are pedagogically more useful than
   abstract ones. When it does, it must (a) be clearly labelled as
   illustrative, and (b) remain a faithful instance of the
   topic-agnostic procedure above it. The example may not introduce
   any procedural step that isn't already in `## Procedure`.
5. **Trigger-explicit.** A skill that fires on no clear trigger is just
   noise. Every skill must list at least one user phrase or one
   coach-detected condition in `triggers`.
6. **Honest dispatch.** If two skills both match a turn, the coach
   chooses one and tells the learner which (so the learner can correct
   the dispatch). The coach should not silently chain multiple skills.
7. **End with a write-back.** The last numbered step in `## Procedure`
   must always be the tracker / session-notes update.

---

## 4. Discovery & dispatch (how the coach uses a skill)

This is the contract from the *coach's* side; skills can rely on it.

1. **Discovery.** At session start, the coach reads `skills/README.md`
   and the front-matter of every `skills/*/SKILL.md`. It builds a small
   in-memory index of `{name, description, triggers, phase}`.
2. **Dispatch.** On every learner turn, before falling into the default
   Socratic loop, the coach checks:
   - Does the message contain any `triggers` phrase verbatim or as a
     close paraphrase?
   - Does the current session state match a coach-detected trigger
     (e.g. "two wrong answers in a row" → consider `error-postmortem`)?
   - If the learner explicitly named a skill ("grill me", "mock exam"),
     dispatch it.
3. **No match.** If nothing matches, the coach uses its default
   Socratic loop from `CLAUDE.md` §1. **The default is never to invoke
   a skill.**
4. **Confirmation.** When dispatching, the coach announces it briefly:
   *"Switching into a quick retrieval-quiz on Domain B — say 'stop' any
   time."* This makes dispatch visible and correctable.
5. **Per-topic opt-in/out.** If `topic-config.md` has a `## 8. Enabled
   Skills` section and lists `disabled:` entries, the coach must not
   dispatch those skills for that topic.
6. **Write-back.** When the skill ends, the coach completes the
   skill's `## Tracker write-back` section as part of the normal
   end-of-session two-step protocol from `CLAUDE.md` §3 / `INIT.md` §2.

---

## 5. Authoring checklist (use this when adding a new skill)

- [ ] Directory name = `name` field = `kebab-case`.
- [ ] Front-matter has all required fields, valid YAML, parseable.
- [ ] `description` reads as *"<verb-phrase>. Use when <condition>."*
- [ ] At least one explicit `triggers` entry.
- [ ] `inputs` and `outputs` reference real files / sections.
- [ ] Body has all six required sections in order.
- [ ] Last step of `## Procedure` is the tracker write-back.
- [ ] No new top-level files or directories created outside
  `skills/<name>/`.
- [ ] Skill is listed in `skills/README.md` under the right phase.
- [ ] Skill is listed (commented-out is fine) in
  `topic-config.template.md` §8 so learners can opt out per topic.
- [ ] Re-run any existing tracker / session example to confirm the
  write-back instructions are unambiguous.

---

## 6. Minimal example skeleton

```markdown
---
name: example-skill
description: >
  Short verb-phrase describing the action. Use when the learner says
  "example phrase" or when <coach-detected condition>.
phase: 5
triggers:
  - "example phrase"
  - learner has just finished a new sub-topic and asks for practice
inputs:
  - topic-config.md: §3 (current sub-topic), §6 (verification level)
  - progress/<tracker>: §"Per-Domain Detail" for the current sub-topic
outputs:
  - session-notes: appends a "Topic" section under "Topics Covered"
  - tracker: may add or close gaps; may mark a sub-topic mastered
verification: inherits
---

## Purpose
…

## When to use
- …

## When NOT to use
- …

## Procedure
1. …
2. …
3. **Tracker write-back** — append <X> to today's session-notes and
   update <Y> in the tracker.

## Verification policy hook
…

## Tracker write-back
- session-notes: …
- tracker: …

## Example
…
```
