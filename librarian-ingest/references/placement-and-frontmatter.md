# Placement & frontmatter reference

Detailed rules for classifying a source document into the Lens, choosing its gbrain `type:`, and
writing ceremony-valid frontmatter. SKILL.md points here before the first placement.

> **The vault's own `LENS.md` is authoritative.** This file is the portable default that ships with
> the skill. If the target canon checkout has a `LENS.md` whose `type:` table differs (a customer may
> have tuned it), follow the vault's `LENS.md` and treat this file as the explanatory backstop.

## 1. Routing: source → folder

Decide the folder from what the document *is about*, not from where it happened to live in the source.

| If the document is about… | Folder | Notes |
|---|---|---|
| Company identity, brand, voice, mission, values | `Context/` | The high-signal "who we are" canon. |
| Ideal customer profile, target market, positioning | `Context/` | |
| Strategy, OKRs, goals, roadmap direction | `Context/` | Direction-setting, not a single project. |
| A specific person (teammate, agent, stakeholder, key customer contact) | `Team/{name}/` | One profile page per person, `Team/{name}/{Name}.md`. |
| A meeting, call, or transcript | `Intelligence/meetings/` | Notes and decisions from a conversation. |
| A competitor, market intel, or a standalone decision/analysis | `Intelligence/` | Non-meeting intelligence. |
| A project (initiative with a goal and scope) | `Projects/{name}/` | One README per project. |
| How work gets done: an SOP, process, playbook | `Departments/{name}/` | Reusable procedure. |
| Reusable content, references, templates, assets | `Resources/` | |
| Onboarding a new hire or new agent | `Onboarding/` | "Getting started" paths. |
| A dated log entry about company activity | `Daily/YYYY-MM-DD.md` | Append to the day's note. |

If a document spans two homes (e.g. a strategy doc that also names a new process), prefer **one primary
page** in the best-fit folder and **`[[wikilink]]`** to a second page rather than duplicating content.

## 2. gbrain `type:` values

`type:` is authoritative for gbrain's retrieval and graph wiring. Use these values (the
`gbrain-base-v2` page types in use in this Lens):

| `type:` | Use for |
|---|---|
| `concept` | Identity, brand, ICP, competitors, market notes, general knowledge. |
| `strategy` | OKRs, goals, direction, roadmap. |
| `person` | A profile of one human or agent. |
| `role` | An operator/agent **role definition** (responsibilities of a seat, vs. `person` = a specific individual). |
| `meeting` | Meeting notes, call transcripts, decisions from a conversation. |
| `project` | A project README / initiative. |
| `process` | An SOP, playbook, or how-to for getting work done. |
| `resource` | Reusable content, references, assets. |
| `guide` | Onboarding / how-to-get-started material. |
| `daily` | A dated company log entry. |

Folder→`type:` defaults (most common pairing):

- `Context/` → `concept` (identity/brand/ICP) or `strategy` (OKRs/direction)
- `Team/` → `person`
- `Intelligence/meetings/` → `meeting`; `Intelligence/` (other) → `concept`
- `Projects/` → `project`
- `Departments/` → `process`
- `Resources/` → `resource`
- `Onboarding/` → `guide`
- `Daily/` → `daily`

If a document genuinely needs a `type:` not in this list, check the vault's `LENS.md` first; only then
introduce a new value, and note it in your report so the convention doc can be updated.

## 3. Required frontmatter

Every page opens with YAML frontmatter:

```yaml
---
type: <one of the values above>     # required — authoritative for gbrain
status: active                      # active | on-hold | completed | archived
tags: [<topic>, <topic>]            # lowercase topical tags
---
```

Conditional fields, added when they apply:

- `date: YYYY-MM-DD` — required for `daily` and `meeting` pages.
- `project: <name>` — for pages under `Projects/`.
- `owner: "[[Name]]"` — when a page has a clear owner.

## 4. Ceremony checklist (every canonical write)

1. **Valid frontmatter** with a `type:` from §2.
2. **No contradiction** with existing canon — especially `Context/`. If your source contradicts canon,
   keep canon and report the conflict; do not overwrite.
3. **`[[wikilinks]]`** for every referenced entity (person, project, company, product).
4. **Clear commit message.**

## 5. House style (matches the vault `CLAUDE.md`)

- **No em dashes** anywhere in vault content.
- **Do not** duplicate the filename as a `# Title` H1 heading.
- **No orphan notes** — every page links in to at least one related page.
- One subject per page; standalone and composable (a lego block, not a dumping ground).
- Direct, specific prose. No hedging. Cite file paths when reporting.
- Never commit secrets (keys, tokens, connection strings).

## 6. Worked examples

**Example 1 — a Notion export of the company's positioning**
Source: `Positioning FINAL v3 a1b2c3.md` (Notion export with `Created:`/`Last edited:` trailer).
- Convert: strip the Notion trailer + UUID from the name.
- Read canon: `mcp__gbrain__search "positioning ICP"` → nothing on point.
- Place: `Context/positioning.md`, `type: concept`.
- Frontmatter: `type: concept`, `status: active`, `tags: [context, positioning, icp]`.
- Link: `[[icp]]`, `[[brand]]`.

```yaml
---
type: concept
status: active
tags: [context, positioning, icp]
---
NestPro wins the [[icp|mid-market property manager]] who...
```

**Example 2 — a PDF of a customer kickoff call**
Source: `2026-05-12 Acme kickoff.pdf`.
- Convert: `pdf` skill → markdown of the transcript.
- Read canon: search `"Acme"` → an `Intelligence/acme.md` competitor note exists, but no meeting page.
- Place: `Intelligence/meetings/2026-05-12-acme-kickoff.md`, `type: meeting`, `date: 2026-05-12`.
- Link the people present as `[[Team/...]]` and the account as `[[acme]]`.

**Example 3 — a contradiction**
Source doc says the company's pricing is usage-based. Canon `Context/pricing.md` says flat-rate.
- **Do not** edit `Context/pricing.md`.
- Report under "Conflicts for review": `acme-pricing.pdf vs Context/pricing.md: source says usage-based,
  canon says flat-rate — needs human adjudication.`

**Example 4 — a duplicate**
Source is a second copy of the team roster already captured at `Context/team.md`.
- Search surfaces `Context/team.md`. Read it; the source adds no new names.
- Skip; report: `team-roster.docx → already covered by Context/team.md (no new facts)`.
