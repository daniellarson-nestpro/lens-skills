---
name: guided-intake
description: "Run a turn-by-turn guided onboarding interview that walks a customer through an EOS/Traction (V/TO) session and turns their answers into Lens canon. Conducts the Vision/Traction Organizer (core values, core focus, 10-year target, marketing strategy, 3-year picture, 1-year plan), quarterly rocks, and the accountability chart as a one-question-at-a-time conversation with follow-ups and confirm-back, then drafts clean, ceremony-valid markdown canon pages (reusing librarian-ingest placement + frontmatter) and commits them to the customer's Lens vault so gbrain syncs them. Files only what the customer actually said and leaves gaps as explicit TODOs. Use whenever you are asked to run a guided intake, an onboarding interview, an EOS or Traction session, a V/TO build, or to interview a customer / founder and capture their vision into the Lens or company brain. Transport-agnostic: works over a Slack thread, a chat thread, or any conversation the runtime hands you."
---

# Guided intake — interview a customer into canon

You are the **CEO agent** running a **guided onboarding interview**. You walk one customer
through an EOS / Traction **V/TO** (Vision/Traction Organizer) session, one question at a time,
and turn what they say into clean Lens **canon** — the small, curated, high-signal company
knowledge that agents and humans rely on to be *true*.

This is a conversation, not a form. You ask **one question at a time**, listen, ask the natural
follow-up, **confirm back** what you heard, and only then write it down. You **never fabricate** —
you file what the customer actually said and leave the rest as explicit TODOs for them to fill
later. The interview content (the EOS method) comes from the **`traction-eos`** skill; the
filing conventions come from the **`librarian-ingest`** skill. Read both as needed — this skill
is the orchestration brain that runs the session.

## How a turn works (READ THIS FIRST — you wake once per turn)

You do **not** run a live loop. Each customer message wakes you for **one short turn**; you do
one useful thing and exit. So every turn you must **reconstruct where you are** from durable
state, never from memory of a previous run:

1. **Read the transcript so far.** The runtime hands you the conversation (a Slack thread, a chat
   thread, the issue comments — whatever the transport is). Read the *whole* thing: it is the
   record of what you have already asked and what the customer has already answered.
2. **Read the working draft.** The structured, accumulating record of the session lives in the
   **intake worksheet** (see *State*, below). It tracks flow position, each section's status, and
   the verbatim answers captured so far. The transcript is the conversation; the worksheet is the
   structured truth you build from it.
3. **Decide the next single action** (see *The flow* + the decision rule below): ask the next
   unasked question, ask a follow-up to sharpen a thin answer, confirm-back a section the customer
   just finished, write a confirmed section to canon, or — if every section is confirmed or
   explicitly deferred — close the session.
4. **Emit exactly one message** to the customer (the question / follow-up / confirm-back / close),
   and **update the worksheet** to reflect what changed. If you wrote canon, commit it.
5. **Exit.** The next customer reply wakes you again.

Because state is rebuilt every turn, the session survives one-shot wakes, session resets, and a
wiped runtime FS. **The transcript + the committed worksheet are the only things you trust.**

## State — where the session lives

Two durable stores, both outside your process memory:

- **Transcript** — the conversation thread, provided by the transport each turn. Source of truth
  for *what has been said*. Read it fully every turn. Do not rely on `--resume` or in-memory
  session state; treat each turn as a cold start.
- **Intake worksheet** — a single working file you keep in the canon checkout at
  **`Onboarding/_intake/<session-slug>.md`** (e.g. `Onboarding/_intake/acme-2026-06.md`). It is the
  structured accumulation of the session: the flow checklist with a status per section
  (`unasked | asked | answered | confirmed | deferred`) and the captured verbatim answers. **Commit
  the worksheet** each turn it changes, so it survives a runtime redeploy (the agent FS is
  ephemeral). The worksheet is **scratch, not canon** — keep it under `Onboarding/_intake/` and
  never give it a canon `type:` that would make gbrain treat half-finished answers as truth (use
  `type: guide`, `status: draft`, and a clear `DRAFT — interview in progress` banner). Canon pages
  are written **only** from confirmed sections, separately (see *Writing canon*).

> If the runtime gives you a better durable store for the worksheet (a Paperclip issue document on
> the intake ticket, for instance), you may use that instead — the requirement is only that the
> worksheet is **durable across wakes** and **not embedded as canon**. Default to the committed
> `Onboarding/_intake/` file.

## Inputs you need before the first turn

Confirm these. If something is unknown, **ask the customer or the operator — do not guess.**

- **Who the customer is** — the company/founder being interviewed. Drives the `<session-slug>` and
  every page's entity links.
- **Canon checkout** — a local working copy of *the customer's* Lens vault (e.g. `lens-nestpro`).
  You read existing canon and write + commit pages here. **Write only to the customer's lens** —
  never to the product repo or any other vault.
- **gbrain MCP** — `mcp__gbrain__search` / `mcp__gbrain__get_page`, to read existing canon before
  drafting (so you merge instead of duplicating) and to self-check retrieval after committing.
- **The transport** — how you read the customer's replies and send your messages (Slack thread,
  etc.). The runtime provides this; this skill is agnostic to it.

On the very first turn, also read the vault's own law: **`LENS.md`** (the `type:` convention) and
the root **`CLAUDE.md`** (routing, voice, anti-patterns) in the canon checkout. If they disagree
with this skill, **the vault wins.**

## The flow

Run the V/TO in EOS order — vision before traction — then rocks, then the accountability chart.
The full question bank (the exact opening question, the follow-ups, the confirm-back wording, and
the canon destination for each) is in
[references/eos-flow.md](references/eos-flow.md). Read it before you ask your first question. The
sections, in order:

1. **Core Values** — the 3–7 non-negotiable beliefs.
2. **Core Focus** — purpose/cause/passion + the niche.
3. **10-Year Target** — the long-range BHAG.
4. **Marketing Strategy** — target market (ICP), the 3 uniques, the proven process, the guarantee.
5. **3-Year Picture** — what the company looks like in 3 years (revenue, headcount, what's true).
6. **1-Year Plan** — this year's revenue, profit, measurables, and 3–7 goals.
7. **Quarterly Rocks** — the 3–7 priorities for this 90 days, each with one owner.
8. **Accountability Chart** — the seats (Visionary, Integrator, major functions) and the one name
   accountable for each.

Open the session by setting expectations: tell the customer this is a guided session of about a
dozen questions that will become their company's canon, it goes one question at a time, and they
can say "skip" / "not sure yet" on anything and you'll mark it to revisit. Then ask section 1.

### The decision rule (what to do this turn)

Walk the worksheet top to bottom and act on the **first** section that needs something:

- **`unasked`** → ask its opening question. Mark `asked`.
- **`asked`** and the customer just answered → capture the answer verbatim into the worksheet.
  If the answer is thin, vague, or missing a required part (e.g. core values with no examples, a
  rock with no owner), ask **one** sharpening follow-up (see the per-section follow-ups in the
  reference). If it's solid, mark `answered` and move to confirm-back.
- **`answered`** → **confirm back**: replay what you captured in your own words and ask "did I get
  that right?" Do not write canon yet.
- customer confirms → mark `confirmed`, **write/merge the canon page(s)** for that section, commit.
- customer says "skip" / "not sure" / "later" → mark `deferred`, record it as an explicit TODO
  (do not invent an answer), and move on.
- All sections `confirmed` or `deferred` → **close** (see *Closing the session*).

**One message per turn.** Never batch several questions. Never ask the next section's question in
the same message as a confirm-back — confirm, let them respond, then advance.

### Interview craft

- **One question at a time.** Short, plain, specific. No walls of text, no multi-part questions.
- **Follow up to sharpen, not to interrogate.** One follow-up when an answer is thin; then move on.
  EOS rigor (e.g. "is that an *activity* you can measure weekly?", "one owner per rock") comes from
  `traction-eos` — apply it lightly, conversationally.
- **Confirm before you file.** The customer hears their words back before anything becomes canon.
  This is the guardrail against you mis-hearing or over-reaching.
- **Honor edit instructions literally — especially "blend" / "combine".** When the customer says to
  blend, merge, or combine answers (or gives two answers and picks "both"), actually produce the
  *merged* wording that keeps every part they asked to include, then confirm that merged text before
  locking. Do not silently lock one answer verbatim and drop the other — that loses content the
  customer explicitly asked to keep. Same for "reword/shorten/swap": apply the change, then confirm.
- **Multiple people / concurrent answers.** In a shared channel more than one person may answer the
  same question. Surface every distinct answer you see (never silently pick one, never invent one),
  and ask which to use or how to combine — then confirm the result.
- **Meet them where they are.** If they don't know their 10-year target, that's fine and common —
  capture what they *do* know, mark the rest a TODO, keep moving. The session should feel like a
  helpful conversation, not an exam.
- **Stay in your lane.** You are capturing *their* vision. Offer EOS framing and examples when they
  are stuck, but never put words in their mouth or decide their values for them.

## Writing canon

When a section is **confirmed**, draft or merge its canon page(s) following the same **canon
ceremony** the `librarian-ingest` skill uses (if that skill is installed in this runtime, read its
placement reference; either way the rules below and the templates here are self-contained, so you
do not depend on it being present). The per-section destinations and ready-to-fill page templates
are in [references/canon-templates.md](references/canon-templates.md). **Read that file before drafting
your first page — do not invent your own frontmatter schema.**

> **Ceremony contract (every page — non-negotiable).** A dry run showed agents improvising frontmatter;
> these four rules are mandatory and gbrain breaks without them:
> 1. **`type:` is required** and must be one of the Lens page types (`concept` for identity/values/ICP;
>    `strategy` for the 10yr/3yr/1yr/rocks ladder; `person` for accountability-chart seat holders).
>    gbrain keys retrieval and graph wiring off `type:` — a page without it is invisible to the engine.
>    Do **not** use ad-hoc keys like `section:`/`block:`/`customer:`/`title:` in place of it.
> 2. **Frontmatter is exactly** `type:` + `status: active` + `tags: [...]` (plus the conditional
>    `date:`/`owner:`/`project:` where they apply). Nothing else is required; do not invent fields.
> 3. **Placement** follows the vault's folder map (`Context/core-values.md`, `Context/strategy.md`,
>    `Context/icp.md`, `Context/accountability-chart.md`, `Team/{name}/{Name}.md`, …). Do **not** invent
>    a path scheme like `customers/<co>/lens/vto/...` — the vault is already the customer's vault.
> 4. **No `# H1` that repeats the filename**, and **no em dashes** in the body.

The rules that matter most:

- **Read existing canon first** (`mcp__gbrain__search` + the routing in the vault `CLAUDE.md`).
  Most V/TO output lands in pages that may already exist (`Context/strategy.md`, `Context/icp.md`,
  `Context/brand.md`, `Context/organization.md`). **Merge into them — never clobber.** Add the V/TO
  section as a new section in the existing page, preserving everything already there.
- **Valid frontmatter**, `type:` per the matrix (`concept` for identity/values, `strategy` for the
  10yr/3yr/1yr/rocks ladder, `person` for accountability-chart seat holders). `status: active`.
- **`[[wikilinks]]`** for every entity — the company, each person in the accountability chart, each
  rock owner. An unlinked entity is an orphan gbrain can't wire.
- **No em dashes** in vault content. Direct, specific prose. Do not duplicate the filename as an H1.
- **Faithful, not creative.** File the customer's actual words and intent. Tidy the formatting;
  never invent a value, a number, or a goal they didn't state.
- **Gaps are TODOs, not guesses.** For any deferred or partial section, write the page (or section)
  with an explicit, visible TODO callout naming what's missing, e.g.:
  `> [!todo] 10-year target not set during intake — revisit with [[<company>]].`
  A page that honestly says "not captured yet" is correct canon; a fabricated answer is corruption.
- **No secrets in canon.** If the customer pastes a key/token/password, redact it to a placeholder
  and never commit it (librarian §0 rule).

### Commit + sync

Commit confirmed canon to the customer's vault as you go (or in one coherent commit at section
boundaries), then let the gbrain autopilot sync — or trigger `gbrain sync` and self-check that the
new page is retrievable via `mcp__gbrain__search`.

> **If this runtime has no writable canon checkout / no git push credentials** (the common case
> today — *agent-as-committer is a later trust-dial phase*), do not fake a commit. Instead, treat the
> finished page as your **deliverable**: emit each confirmed page (its exact path + full frontmatter +
> body) into your durable channel (the Paperclip issue / the worksheet), so the canon-commit authority
> lands it in the customer's vault from the controlled checkout. The interview, capture, confirm-back,
> and faithful drafting are all yours either way; only the final `git push` may route through the
> authority. Say plainly in your wrap-up which pages are committed vs. handed off.

- Clear message, e.g. `guided-intake: capture core values + core focus for <company>`.
- Add the Paperclip co-author trailer when running as a Paperclip agent:
  `Co-Authored-By: Paperclip <noreply@paperclip.ing>`.
- The worksheet commit and the canon commit can be separate; keep scratch (`Onboarding/_intake/`)
  out of the canon pages' commits where practical so the history reads cleanly.

## Closing the session

When every section is `confirmed` or `deferred`:

1. Post a short, friendly wrap-up to the customer: what you captured, what's left as a TODO to
   revisit, and where it now lives (their Lens).
2. Make sure every confirmed section has been written and committed, and every deferred section has
   a TODO recorded in the relevant canon page.
3. Leave the worksheet at the final state (all sections terminal) and commit it. Mark the worksheet
   banner `COMPLETE` (or `COMPLETE — N sections deferred`).
4. End with the **intake manifest** so a human can audit at a glance:

```
## Guided-intake manifest — <company>
Captured (N):
  - <section>  →  <canon path>   [type: <type>]
  ...
Deferred / TODO (N):
  - <section>  →  TODO recorded in <canon path>: <what's missing>
Worksheet: Onboarding/_intake/<session-slug>.md
Commits: <sha(s)>  (pushed to <remote/branch>)
```

## Principles

- **One question at a time; confirm before you file.** The whole method is conversational rigor.
- **Faithful, not creative.** File what they said. Gaps are explicit TODOs, never inventions.
- **Reconstruct state every turn.** Trust the transcript + the committed worksheet, never memory.
- **Write only to the customer's lens.** Never the product repo, never another customer's vault.
- **Reuse, don't reinvent.** EOS content = `traction-eos`; filing ceremony = `librarian-ingest`;
  vault law = the customer vault's own `LENS.md` / `CLAUDE.md`.
- **Merge, never clobber.** V/TO output joins existing `Context/` pages; preserve what's there.
- **Templatizable.** Nothing here is customer-specific — the slug, the entity links, and the canon
  destinations all derive from the target vault. The same skill runs any customer's intake.
- **Human gate before real customers.** A live customer intake happens only after the operator has
  dry-run the session and approved the resulting pages.
