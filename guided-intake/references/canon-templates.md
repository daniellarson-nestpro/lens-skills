# Canon page templates

Ready-to-fill templates for each section the intake produces. Fill **only** with what the customer
confirmed; leave anything they didn't give as a visible TODO callout (never a guess). All pages
follow `librarian-ingest` ceremony and the vault's `LENS.md`. Read the vault's own `LENS.md` /
`CLAUDE.md` first; if they differ, the vault wins.

Universal rules:
- Valid YAML frontmatter with a `type:` from the matrix; `status: active`.
- `[[wikilinks]]` for every entity (the company, every person).
- **No em dashes** in vault content, and no en-dash ranges: write "3 to 7", not "3-7". The templates
  below are already em-dash-free; keep them that way when you fill them in.
- No `# Title` H1 that duplicates the filename. No orphan pages.
- **Read existing canon first.** Pages marked *(merge)* below usually already exist. Add your
  section, preserve everything there, do not clobber.
- TODO for any missing/deferred piece, em-dash-free:
  `> [!todo] <what> not captured during intake. Revisit with [[{company}]].`

The intake worksheet itself is **scratch, not canon**: keep it at `Onboarding/_intake/<slug>.md`
with `type: guide`, `status: draft`, and a `DRAFT, interview in progress` banner so gbrain never
treats half-finished answers as truth.

---

## 1. Core Values, `Context/core-values.md` (new)

```markdown
---
type: concept
status: active
tags: [context, core-values, eos, vto]
---

[[{company}]]'s core values: the non-negotiable beliefs the company hires, fires, and decides by.

## {Value 1}
{customer's own one-line description / example}

## {Value 2}
{...}

> [!info] Captured via guided intake (EOS V/TO) on {date}.
```

---

## 2. Core Focus, `Context/core-focus.md` (new)

```markdown
---
type: concept
status: active
tags: [context, core-focus, eos, vto]
---

[[{company}]]'s core focus: the purpose behind the company and the one thing it is best at.

**Purpose / cause / passion:** {purpose, the customer's words}

**Niche:** {niche, one phrase}

Everything [[{company}]] does should serve this niche and express this purpose. Ties to
[[core-values]] and [[strategy]].
```

---

## 3 / 5 / 6. Traction ladder, `Context/strategy.md` (merge, likely exists)

Add these as **sections** in the existing `Context/strategy.md`. If the file does not exist, create
it with the frontmatter shown. Keep any existing strategy content above/around these sections.

```markdown
---
type: strategy
status: active
tags: [context, strategy, eos, vto]
---

[[{company}]]'s vision and traction ladder, from the EOS V/TO.

## 10-Year Target
{target statement}, by ~{year}.
<!-- or, if deferred: -->
> [!todo] 10-year target not set during intake. Revisit with [[{company}]].

## 3-Year Picture
- Revenue: {revenue}
- Team: {headcount}
- What's true: {qualitative bullets in the customer's words}

## 1-Year Plan
- Revenue: {revenue} · Profit: {profit}
- Measurables: {key weekly/monthly numbers}
- Goals ({n}): {the 3 to 7 goals}
```

When a section is deferred, write the heading and a TODO callout under it rather than omitting it,
so the structure is visible and the gap is explicit.

---

## 4. Marketing strategy, `Context/icp.md` + `Context/brand.md` (merge, may exist)

**Target market, `Context/icp.md`** (merge; create if absent):

```markdown
---
type: concept
status: active
tags: [context, icp, marketing, eos]
---

[[{company}]]'s ideal customer profile, from the EOS V/TO marketing strategy.

**Ideal customer:** {ICP definition: industry, size, role, defining traits}
```

**3 uniques + proven process + guarantee, `Context/brand.md`** (merge; create if absent):

```markdown
---
type: concept
status: active
tags: [context, brand, positioning, eos]
---

What makes [[{company}]] different, from the EOS V/TO marketing strategy.

## Three uniques
1. {unique 1}
2. {unique 2}
3. {unique 3}

## Proven process
{named, repeatable delivery process}
<!-- or -->
> [!todo] Proven process not defined during intake. Revisit with [[{company}]].

## Guarantee
{the promise/guarantee}
<!-- or -->
> [!todo] Guarantee not defined during intake. Revisit with [[{company}]].
```

---

## 7. Quarterly Rocks, `Context/rocks-{YYYY}-Q{n}.md` (new, one per quarter)

```markdown
---
type: strategy
status: active
tags: [context, rocks, eos, traction]
---

[[{company}]]'s rocks for {Quarter YYYY}: the 3 to 7 priorities for these 90 days. Each has one
owner and a clear done.

| Rock | Owner | Done when |
|------|-------|-----------|
| {rock 1} | [[{owner}]] | {done-criterion} |
| {rock 2} | [[{owner}]] | {...} |

Reviewed weekly against the [[strategy|1-year plan]]. Owners are seats on the
[[accountability-chart]].
```

Each `{owner}` must be a `[[wikilink]]` to a `Team/{name}/{Name}.md` page (create a stub if needed,
see section 8).

---

## 8. Accountability Chart, `Context/accountability-chart.md` (new) + `Team/{name}/{Name}.md` stubs

**The chart, `Context/accountability-chart.md`:**

```markdown
---
type: concept
status: active
tags: [context, accountability-chart, people, eos]
---

[[{company}]]'s accountability chart: the seats and the one person accountable for each. One name
per seat (EOS rule).

- **Visionary:** [[{name}]]
- **Integrator:** [[{name}]]
- **Sales / Marketing:** [[{name}]]
- **Operations:** [[{name}]]
- **Finance:** [[{name}]]
- {other seats as given}

> [!todo] {any seat with no name}: owner not assigned during intake.
```

**Each named person, `Team/{name}/{Name}.md`** (create a stub only if they aren't already in
`Team/`; never clobber an existing profile):

```markdown
---
type: person
status: active
tags: [team, person]
---

[[{Name}]]: {one-line role at [[{company}]], from the customer's words}.

Accountable for {seat(s)} on the [[accountability-chart]].
```

Keep stubs minimal and faithful: one line the customer actually said. Do not invent bios, titles,
or contact details.
