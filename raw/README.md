# `raw/` — Source Templates for Per-Topic Source-of-Truth Material

This directory holds **only the templates** used to seed each topic's
own `topics/<short-code>/raw/` directory. Real raw assets never live
here; they live under the topic that owns them.

```
raw/
  README.md                ← you are here
  RAW-INDEX-TEMPLATE.md    ← source template for per-topic raw/INDEX.md
  META-TEMPLATE.md         ← source template for each asset's .meta.md

topics/<short-code>/
  raw/                     ← actual source-of-truth material for this topic
    INDEX.md               ← directory page (mandatory, generated from template)
    user/                  ← user-submitted originals (PDFs, books, slides…)
      <slug>.<ext>
      <slug>.meta.md
    web/                   ← AI-fetched web/document snapshots
      YYYY-MM-DD-<slug>.md
      YYYY-MM-DD-<slug>.meta.md
    derived/               ← optional summaries / extracts built from the above
      <slug>.md
```

---

## Why a `raw/` folder per topic?

The repo already captures three kinds of artifacts per topic:

- **`tracker.md`** — the *map*: what's mastered, what's open, what to
  study next.
- **`sessions/YYYY-MM-DD.md`** — the *log*: what happened in each
  conversation, in chronological order.
- **`notes/<domain>-…md`** — the *reference*: distilled definitive
  knowledge organized by syllabus.

What was missing is the *bedrock* the reference rests on: an
**immutable, in-repo copy** of the original source material every
factual claim ultimately points to. That is `raw/`.

Two practical wins:

1. **Anti-hallucination.** Every concrete fact written into chapter
   notes can — and under `strict` verification level **must** —
   reference a specific file under `raw/`. The agent cannot invent
   facts that do not appear in any raw file.
2. **Time-stable references.** Web pages move and change. A snapshot
   under `raw/web/2026-05-01-<slug>.md` is the same tomorrow as it is
   today, so notes never drift out from under their citations.

## What goes in (and what does not)

**In** — *original* content the topic depends on:

- User-submitted study guides, textbook chapters, slide decks, course
  transcripts (`raw/user/`).
- AI-fetched snapshots of authoritative web pages (`raw/web/`).
- Optional derived extracts the coach has built **from** the above
  (`raw/derived/`) — e.g. "Section 4 of the IRS publication, quoted
  verbatim, with the table extracted as markdown".

**Out** — anything that belongs in another artifact:

- The coach's distilled definitions or paraphrases → **chapter notes**
  (`topics/<short-code>/notes/`).
- Conversation, comprehension checks, learner's wording → **session
  notes** (`topics/<short-code>/sessions/`).
- Mastery state, gaps, next-step planning → **tracker**.
- Cross-topic insights → **`crosslinks/`**.

The hard rule for the AI: **`raw/` files are only original quoted
content plus the `.meta.md` sidecar. The agent never adds its own
interpretation inside a raw file** — that belongs in chapter notes.

## Immutability

Files under `raw/user/` and `raw/web/` are **append-only snapshots**.
Once written they are not edited in place. To "update" a web snapshot,
fetch again and create a new `YYYY-MM-DD-<slug>.md` file with today's
date; the older snapshot stays so historical citations remain valid.

`raw/derived/` is editable (it is the coach's working space), but each
derived file must reference the specific `user/` or `web/` files it
was built from.

## Granularity

- One file per source. A multi-chapter PDF the user submits stays as
  one file in `user/`; per-chapter quoted extracts go in `derived/`
  with cross-references back.
- One snapshot per fetch. If the same page is re-fetched a month
  later, that's a new dated file, not an overwrite.

## When the coach updates `raw/`

See [`../CLAUDE.md`](../CLAUDE.md) §4 for the full retrieve → archive →
cite loop. Short version: **whenever the coach searches the web for a
factual claim, it must save the relevant excerpt to
`topics/<short-code>/raw/web/YYYY-MM-DD-<slug>.md`, write a sidecar
`.meta.md`, and append a row to `topics/<short-code>/raw/INDEX.md`
before quoting the claim back to the learner.** The same loop applies
when the learner uploads a new file: it goes in `raw/user/` with a
sidecar and an `INDEX.md` row.

## Initialization

When a new topic is added (`INIT.md` §1.2), the coach creates
`topics/<short-code>/raw/{user,web,derived}/` and seeds an empty
`raw/INDEX.md` from `RAW-INDEX-TEMPLATE.md`. The folders are empty
until real material arrives.

## Verification policy interaction

Raw assets are the **substrate** the verification policy operates on:

- `strict` → every concrete fact in `notes/` must cite a `raw/` file
  path; the coach fetches first and notes second.
- `standard` → cite `raw/` files where they exist; fetching is
  expected for specific dates / numbers / regulations.
- `relaxed` → `raw/` is optional; it still functions as the source of
  truth when the learner asks to defend a claim.

See [`../CLAUDE.md`](../CLAUDE.md) §4 for the full mapping between
verification level and **Web Search Policy** (`proactive` /
`on-demand` / `off`).

## File-size & copyright defaults

The repo-root `.gitignore` ignores common large/binary file types
(`*.pdf`, `*.epub`, `*.mp4`, `*.mov`, `*.mp3`, `*.zip`) under
`topics/*/raw/user/` by default. The accompanying `*.meta.md` and any
markdown extract in `raw/derived/` are still tracked, so the
provenance of a file remains in git even when the binary itself stays
local. To intentionally share a binary, `git add -f path/to/file.pdf`.

`raw/web/` is **not** ignored — those are short markdown excerpts
fetched for personal study and benefit from being version-controlled.

The `META-TEMPLATE.md` includes a `license:` field, defaulting to
`unknown — quoted for personal study`. Please review and update it
before sharing the repo publicly.
