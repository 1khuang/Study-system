# {{Topic Name}} — Raw Source Index

> Directory page for **all** source-of-truth material this topic
> references. Copy this template to
> `topics/<short-code>/raw/INDEX.md` when initializing the topic
> (`INIT.md` §1.2). The coach reads this file at the start of every
> session (see `CLAUDE.md` §0) so it knows which `raw/` files already
> exist and can prefer them over re-searching the web.
>
> **Hard rule:** every file under `raw/user/`, `raw/web/`, and
> `raw/derived/` must have a row here. If you add a file by hand,
> append the row yourself; the coach will add a row whenever it
> writes a new raw asset.

- **Topic**: {{Topic Name}} (`{{short-code}}`)
- **Last Updated**: {{YYYY-MM-DD}}
- **Verification Level** (from `topic-config.md`): {{strict | standard | relaxed}}
- **Web Search Policy** (from `topic-config.md` §6): {{proactive | on-demand | off}}

---

## How to read this index

Each row points at a file under `topics/<short-code>/raw/`.

- **File** — relative path from this `INDEX.md` (e.g. `./web/2026-05-01-irs-pub-590a.md`).
- **Source** — for `user/`, who supplied it; for `web/`, the original URL; for `derived/`, the parent file.
- **Date** — upload date for `user/`, fetch date for `web/`, build date for `derived/`.
- **Bound to** — the `<domain-code>.<sub-topic-code>` codes from
  `topic-config.md` §3 this asset is authoritative for. A single asset
  can cover multiple sub-topics.
- **Notes** — one-line description, e.g. "Chapter 4 only" or
  "Contribution-limit table".

When the coach answers a question, it should prefer assets in this
order: `user/` → `web/` → re-search.

---

## 📥 User-Submitted Originals (`raw/user/`)

| File | Source | Date | Bound to | Notes |
|------|--------|------|----------|-------|
| _none yet_ |  |  |  |  |

## 🌐 Web Snapshots (`raw/web/`)

| File | Source URL | Fetched | Bound to | Notes |
|------|------------|---------|----------|-------|
| _none yet_ |  |  |  |  |

## 🧩 Derived Extracts (`raw/derived/`)

| File | Built from | Built on | Bound to | Notes |
|------|------------|----------|----------|-------|
| _none yet_ |  |  |  |  |

---

## Coverage Snapshot

> Quick view of how many raw assets each domain currently has. Mirrors
> the **Source Coverage** section in `tracker.md` and is recomputed by
> the coach after every session in which a raw asset was added.

| Domain | User | Web | Derived | Total |
|--------|-----:|----:|--------:|------:|
| {{A. Domain Name}} | 0 | 0 | 0 | 0 |
| {{B. Domain Name}} | 0 | 0 | 0 | 0 |
