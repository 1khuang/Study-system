# Session Notes — {{YYYY-MM-DD}}

> Copy this template to `topics/<short-code>/sessions/{{YYYY-MM-DD}}.md`
> (one file per study day, **per topic**). If the same day's file already
> exists, **edit** it — never overwrite. Cross-topic insights belong in
> [`crosslinks/`](../crosslinks/), not in this file.

## Session Overview
- **Date**: {{YYYY-MM-DD}}
- **Topic**: {{Topic Name from topics/<short-code>/topic-config.md}}
- **Duration**: {{e.g. ~45 min}}
- **Format**: {{e.g. Q&A / worked examples / quiz / spaced review}}
- **Main Focus**: {{1–2 lines on what this session was about}}
- **Days Until Target Date**: {{N or N/A}}

---

## Session Context

**Learner Request**: {{What the learner asked for at the start}}

**Linked Domains / Sub-topics** (from `topics/<short-code>/topic-config.md`):
- {{e.g. B.10 Financing strategies and debt management}}
- {{e.g. F.51 Distribution rules and taxation}}

---

## Topics Covered

### Topic 1: {{name}}

- **Question(s) asked**: {{verbatim if reasonable}}
- **Initial understanding**: {{what learner knew going in}}
- **Explanation given**: {{short summary of what was taught}}
- **Sources cited**: {{links / pages — required if config verification level is `strict`. For `strict`, list **both** the in-repo `raw/` path (primary) **and** the original URL (secondary), per `CLAUDE.md` §4.1.}}
- **Comprehension check**: {{question posed → learner's answer → assessment}}
- **Result**: {{Mastered / Partial / Needs more work}}

### Topic 2: {{name}}
{{Same structure as above. Repeat as needed.}}

---

## Knowledge Gap Changes

> Standardized record of how the gap list moved this session. The coach uses
> this to update the Knowledge Gaps tables in the tracker.

### 🆕 New Gaps Identified
| Gap | Domain | Severity (H/M/L) | Why it's a gap |
|-----|--------|------------------|----------------|
|     |        |                  |                |

### ✅ Gaps Resolved
| Gap | Severity Before | How it was closed |
|-----|-----------------|-------------------|
|     |                 |                   |

### 🔁 Gaps Still Open (Touched This Session)
| Gap | Progress made | Next step |
|-----|---------------|-----------|
|     |               |           |

---

## Sub-topics Newly Mastered

| Sub-topic (code + name) | Confidence (H / M-H / M) | Key points understood |
|-------------------------|--------------------------|-----------------------|
|                         |                          |                       |

## Sub-topics Touched but Not Yet Mastered

| Sub-topic (code + name) | What's understood | What still needs work |
|-------------------------|-------------------|-----------------------|
|                         |                   |                       |

---

## Key Insights / Memorable Moments
- {{e.g. learner spotted an inconsistency in the question wording}}
- {{e.g. analogy that finally made the concept click}}

---

## Chapter Notes Updated

> List every file under `topics/<short-code>/notes/` that was edited
> this session, with a one-line summary of what changed. Leave the
> table empty if no definitive knowledge surfaced today (pure
> conversation / comprehension checks). See `CLAUDE.md` §3a and
> `INIT.md` §2b.

| Notes file | Sub-topic(s) | What changed |
|------------|--------------|--------------|
| {{../notes/A-foundations.md}} | {{A.3}} | {{Added definition of X and the 4 conditions for Y}} |

---

## Raw Assets Added

> List every file added under `topics/<short-code>/raw/` this session
> — both learner uploads (`raw/user/`) and AI-archived web fetches
> (`raw/web/`), plus any derived extracts. Each new asset must also
> have a sibling `.meta.md` and a row in `raw/INDEX.md`. Leave the
> table empty if nothing was added today. See `CLAUDE.md` §4.1 and
> `INIT.md` §2c.

| Raw file | Type (user / web / derived) | Source (URL or uploader) | Bound to sub-topic(s) |
|----------|-----------------------------|--------------------------|-----------------------|
| {{../raw/web/2026-05-01-irs-pub-590a.md}} | web | https://www.irs.gov/... | {{F.51}} |

---

## Action Items for Next Session

- [ ] **Review**: {{topics to revisit}}
- [ ] **Practice**: {{problems / drills}}
- [ ] **Explore**: {{new sub-topics suggested by today's gaps}}

---

## Tracker Update Checklist

> The coach must complete this before ending the session.
> See `INIT.md` §2 for the full daily-update workflow.

- [ ] Updated **Last Updated** date in tracker
- [ ] Updated **Quick Stats** counts
- [ ] Moved newly mastered sub-topics into the right domain section
- [ ] Updated **Knowledge Gaps** tables (new / resolved / touched)
- [ ] Recomputed **Domain Progress Summary**
- [ ] Recomputed **Next-Step Study Plan**
- [ ] Appended row to **Session Index**
- [ ] Updated **Chapter Notes** for any definitive knowledge surfaced
      this session (and recorded touched files above)
- [ ] Updated `raw/INDEX.md` and **Source Coverage** (in the tracker)
      for any new raw assets added this session (recorded above)
