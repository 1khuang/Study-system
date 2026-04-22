# {{Topic Name}} — Study Tracker

> Generated from `topic-config.md`. This is the **single source of truth** for
> overall progress on this topic. The AI study coach updates it after every
> session: domain progress, mastered sub-topics, knowledge gaps, and the
> next-step study plan.
>
> Detailed per-session records live in `/sessions/YYYY-MM-DD/session-notes.md`.

- **Last Updated**: {{YYYY-MM-DD}}
- **Topic**: {{Topic Name}}  ({{short-code}})
- **Started**: {{YYYY-MM-DD}}
- **Target Date**: {{YYYY-MM-DD or "Ongoing"}}
- **Days Remaining**: {{N}}

---

## Quick Stats

- 📊 **Overall Progress**: {{covered}}/{{total}} sub-topics covered = **{{pct}}%**
- 🟢 **Mastered**: {{mastered_count}}
- 🟡 **In Progress**: {{in_progress_count}}
- 🔴 **Open Knowledge Gaps**: {{gap_count}}
- 🗓️  **Sessions Logged**: {{sessions_count}}

---

## Domain Progress Summary

> Sorted by **weight × (1 − coverage)** so the highest-leverage areas surface
> first. The "Priority" column is recomputed after each session.

| Domain | Weight | Covered | Mastered | Status | Priority |
|--------|-------:|--------:|---------:|--------|----------|
| {{A. Domain Name}} | {{w}}% | {{c}}/{{n}} | {{m}}/{{n}} | ⚪ Not Started / 🟡 In Progress / 🟢 Complete | High / Medium / Low |
| ...    |        |         |          |        |          |

---

## Per-Domain Detail

> One section per domain from `topic-config.md`.
> Each sub-topic is one of:
> - `[ ]` not yet covered
> - `[~]` introduced but not mastered
> - `[x]` mastered (date + confidence + key points)

### {{A. Domain Name}} ({{weight}}%)

**Materials**: {{pointer to relevant chapters/pages}}

#### ✅ Mastered
- [x] **{{A.1 sub-topic}}** ({{YYYY-MM-DD}}) — confidence: **High / Medium-High / Medium**
  - Key point: ...
  - Key point: ...
  - Source: {{material reference}}

#### 🟡 In Progress
- [~] **{{A.2 sub-topic}}** — last touched {{YYYY-MM-DD}}
  - What's understood: ...
  - What still needs work: ...

#### ⚪ Not Yet Studied
- [ ] **{{A.3 sub-topic}}**

---

## Knowledge Gaps

> Tracked by severity. Gaps move from **Open → Recently Resolved** when a
> session demonstrably closes them.

### 🔴 High Severity (close ASAP)
| Gap | Domain | First Seen | Sessions Touched | Notes |
|-----|--------|-----------:|-----------------:|-------|
|     |        |            |                  |       |

### 🟡 Medium Severity
| Gap | Domain | First Seen | Sessions Touched | Notes |
|-----|--------|-----------:|-----------------:|-------|
|     |        |            |                  |       |

### 🟢 Low Severity
| Gap | Domain | First Seen | Sessions Touched | Notes |
|-----|--------|-----------:|-----------------:|-------|
|     |        |            |                  |       |

### ✅ Recently Resolved
| Gap | Resolved On | Resolved In Session | How |
|-----|-------------|---------------------|-----|
|     |             |                     |     |

---

## Next-Step Study Plan

> Recomputed after each session from: weights, coverage, open gaps, and time
> remaining. Top of the list is what to do next.

1. **{{Domain / sub-topic}}** — _why: {{e.g. high weight + low coverage + open gap}}_
2. **{{Domain / sub-topic}}** — _why: ..._
3. **{{Domain / sub-topic}}** — _why: ..._

### Milestones (from topic-config.md)
- [ ] {{YYYY-MM-DD: milestone}}
- [ ] {{YYYY-MM-DD: milestone}}

---

## Session Index

| Date | Notes | Main Topics | New Mastered | New / Resolved Gaps |
|------|-------|-------------|--------------|---------------------|
| {{YYYY-MM-DD}} | [link](../sessions/{{YYYY-MM-DD}}/session-notes.md) |  |  |  |
