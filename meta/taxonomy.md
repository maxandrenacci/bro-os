---
type: meta
last_updated: 2026-04-10
---

# Tag Taxonomy — Living Registry

> This is the authoritative registry of all tags in use in bro-os.
> **Type tags are fixed** (defined in CLAUDE.md §6) and can never be modified without a system rule change.
> **Theme tags are emergent** — Bro proposes them during daily processing, registers them here, and they evolve via weekly refactoring.

---

## Type tags (FIXED — never modify)

| Tag | Purpose |
|---|---|
| `#product` | A product owned or tracked by the user |
| `#person` | A stakeholder, colleague, external contact |
| `#project` | Time-bounded effort with deliverable and end date |
| `#area` | Continuous responsibility, no end date |
| `#regulation` | Law, directive, RTS, guideline, authority circular |
| `#decision` | A decision taken, with rationale |
| `#meeting` | Meeting minutes / outcome |
| `#concept` | A concept/definition (e.g., take_rate, passporting) |
| `#moc` | Map of Content — thematic synthesis note |
| `#output` | Generated artifact meta-file |
| `#resource` | Source document meta-file (in `/resources/`) |

## State tags (FIXED, for projects only)

| Tag | Meaning |
|---|---|
| `#active` | In progress |
| `#paused` | Temporarily on hold |
| `#done` | Completed |
| `#archived` | Closed and no longer relevant |

## Workflow tags (FIXED — used by the processing pipeline)

> These tags are structural to the daily/weekly pipeline. They are not content classifiers — they mark tasks for follow-up and triage. Never use them as theme tags.

| Tag | Used by | Purpose |
|---|---|---|
| `#follow-up` | Daily processing | Marks to-dos created by Bro during enrichment that need user verification. Surfaced in `meta/todos.md` follow-up query. |

---

## Theme tags (EMERGENT — managed by Bro, evolves over time)

> Bro adds rows here when introducing a new theme tag during daily processing. Weekly refactoring proposes merges, splits, eliminations.
> **Starting tags below were defined at first daily.** File counts reset to 0 at template publication — they will grow from real use.

| Tag | First seen | File count | Last used | Operational definition |
|---|---|---|---|---|
| `#economics` | — | 0 | — | Financial analysis, margins, ROE, unit economics, P&L. Broad domain — prefer over narrow specifics like #margin. |
| `#research` | — | 0 | — | Customer research, UX research, user interviews, JTBD framework. |
| `#legal` | — | 0 | — | Licensing, regulatory requirements, compliance, legal structure, regulatory filings. |
| `#analytics` | — | 0 | — | Data analytics, BI tools, business analytics. |

---

## Tag hygiene rules

1. New theme tag is introduced **only** when an existing one doesn't fit and the concept is recurring.
2. Every new theme tag is **explicitly declared** in the daily session report with rationale.
3. Theme tag with <3 files after 2 weeks → candidate for elimination at next weekly.
4. Theme tag with >30 files → candidate for sub-tag splitting at next weekly.
5. Two theme tags with semantic overlap → candidate for merge at next weekly.
6. **Default to broad domains over narrow specifics.** `#economics` is a domain. `#margin` is a specific. Specifics earn their existence empirically.

## Change log

| Date | Change | Rationale |
|---|---|---|
| 2026-04-07 | Initial taxonomy created | System bootstrap |
| 2026-04-07 | Added `#resource` type tag | v1.3 — introduction of /resources folder |
| 2026-04-10 | Added `#follow-up` workflow tag | Structural to daily pipeline — needed in todos.md query |
| 2026-04-10 | Cleaned theme tags for template publication — reset file counts to 0 | GitHub template release |
