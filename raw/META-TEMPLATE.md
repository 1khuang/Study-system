# {{slug}} — Asset Metadata

> Sidecar for a single raw asset. Copy this template to
> `topics/<short-code>/raw/<user|web|derived>/<filename>.meta.md`,
> sitting next to the asset file it describes. Both files share the
> same base name; only the extension differs (`foo.pdf` ↔
> `foo.meta.md`, `2026-05-01-irs-pub-590a.md` ↔
> `2026-05-01-irs-pub-590a.meta.md`).
>
> Every raw asset **must** have a `.meta.md`. The coach uses these
> sidecars when deciding whether a fact in chapter notes is properly
> sourced.

---

## Identity

- **Asset file**: {{relative path to the asset, e.g. ./2026-05-01-irs-pub-590a.md}}
- **Kind**: {{user | web | derived}}
- **Slug**: {{short-slug-used-in-filename}}
- **Topic**: {{Topic Name}} (`{{short-code}}`)
- **Bound to** (sub-topic codes from `topic-config.md` §3):
  - {{e.g. F.51, F.52}}

## Provenance

> Fill in only the fields that apply to this asset's kind.

### For `user/` (learner-supplied)

- **Submitted by**: {{learner handle / "self"}}
- **Submitted on**: {{YYYY-MM-DD}}
- **Original title**: {{e.g. "Official CFP Study Guide, 9th ed., Ch. 4"}}
- **Author / publisher**: {{}}
- **Edition / version / date**: {{}}

### For `web/` (AI-fetched)

- **Source URL**: {{full URL}}
- **Fetched at**: {{YYYY-MM-DDTHH:MM:SSZ}}
- **HTTP status**: {{e.g. 200}}
- **Fetched by**: {{agent name / tool used}}
- **Excerpt scope**: {{e.g. "Section 4 only" / "full page"}}

### For `derived/`

- **Built from**: {{relative path(s) to the source asset(s)}}
- **Built on**: {{YYYY-MM-DD}}
- **How**: {{one-line description of the transformation, e.g. "table 3 extracted as markdown"}}

## Integrity

- **SHA-256**: {{`sha256sum <asset>` output, lowercase hex}}
- **Size (bytes)**: {{}}

## License & Use

- **License**: {{e.g. "Public domain", "CC-BY-4.0", or default: "unknown — quoted for personal study"}}
- **Sharing**: {{"OK to commit to public repo" | "local only — do not share"}}
- **Notes**: {{any copyright caveat the learner should know before sharing the repo}}

## Touched In

> One line per session note that referenced this asset. The coach
> appends a row whenever it cites this asset in a session.

- {{../../sessions/YYYY-MM-DD.md}} — {{one-line what was cited}}
