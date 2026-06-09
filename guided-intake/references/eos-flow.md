# EOS / V/TO question bank

The turn-by-turn script for the guided intake. One section at a time, one question at a time.
For each section: the **opening question** (plain language), the **follow-ups** you reach for when
an answer is thin, the **confirm-back** template, and the **canon destination**. The EOS method
behind these lives in the `traction-eos` skill; this is the conversational front end.

House rules (from SKILL.md): ask one question per turn; follow up at most once to sharpen; confirm
back before filing; capture verbatim; gaps become explicit TODOs, never inventions.

Notation: `{company}` = the customer's company; `{name}` = a person; replace at runtime.

---

## Opening (first turn)

> "I'm going to walk you through a short guided session — about a dozen questions — that becomes
> your company's canon: the core of what your agents and team will rely on as true. We'll go one
> question at a time. If you don't know an answer yet, just say 'skip' and I'll mark it to come back
> to. Ready? First one:"

Then immediately ask Section 1.

---

## 1. Core Values  →  `Context/core-values.md` (`type: concept`)

**Opening:** "What are the core values your company runs on — the 3 to 7 beliefs you'd hire, fire,
and make decisions by, even when it costs you?"

**Follow-ups (one, if thin):**
- If they give vague words ("integrity", "excellence") with no meaning: "Give me a quick example of
  what '{value}' looks like in practice here — what does someone do that proves it?"
- If they list more than 7: "That's a lot to live by. Which 3 to 7 are truly non-negotiable?"
- If they give zero: "Think of your best person. What do they do that you wish everyone did? That's
  often a core value."

**Confirm-back:** "So your core values are: {list}. Each one a real filter you'd hold the line on.
Did I get those right?"

**Capture:** the value names + the customer's own one-line description/example for each.

---

## 2. Core Focus  →  `Context/core-focus.md` (`type: concept`)

**Opening:** "Why does {company} exist — the purpose or cause behind it — and what's the one thing
you're truly best at (your niche)?"

**Follow-ups (one, if thin):**
- If only purpose, no niche: "And the niche — in one phrase, what's the thing you do better than
  anyone, that everything else supports?"
- If only niche, no purpose: "Behind the work, what's the deeper reason — the cause or passion that
  keeps you doing this?"

**Confirm-back:** "Your core focus: your purpose is '{purpose}', and your niche is '{niche}'. Right?"

**Capture:** purpose/cause/passion (one line) + niche (one phrase).

---

## 3. 10-Year Target  →  merge into `Context/strategy.md` `## 10-Year Target` (`type: strategy`)

**Opening:** "Look out 10 years. What's the one big, audacious target that says you've made it — a
number or a clear, concrete picture?"

**Follow-ups (one, if thin):**
- If vague ("be the best"): "Make it concrete — a revenue number, a customer count, a market
  position. What would you point to and say 'we did it'?"
- If they don't know: "Totally common. Want to ballpark it, or skip and revisit?" (If skip →
  `deferred`, TODO.)

**Confirm-back:** "Your 10-year target: {target}, by roughly {year}. Got it right?"

**Capture:** the target statement + the target year if given.

---

## 4. Marketing Strategy  →  `Context/icp.md` (target market) + `Context/brand.md` (uniques, guarantee) (`type: concept`)

This one has four parts. Ask them as a short sequence — still one message per turn — not all at once.

**4a. Target market (ICP):** "Who's your ideal customer — the specific kind of buyer you're built
to win? Industry, size, role, whatever defines them."
- Follow-up if broad ("everyone"): "If you could only serve one type of customer, who'd make you
  the most money with the least friction?"

**4b. The 3 uniques:** "What are the 3 things that make you different — the reasons your ideal
customer picks you over the alternatives?"
- Follow-up if generic: "Would a competitor honestly claim the same thing? If yes, push for what's
  truly yours."

**4c. Proven process:** "Do you have a named, repeatable way you deliver — the steps a customer goes
through with you?" (If none: capture "not yet defined" → TODO, don't invent one.)

**4d. Guarantee:** "Is there a promise or guarantee you make to customers?" (If none: TODO.)

**Confirm-back:** "Your marketing strategy — ideal customer: {ICP}; your 3 uniques: {1,2,3}; proven
process: {process}; guarantee: {guarantee}. Accurate?"

**Capture:** ICP definition; the three uniques verbatim; the process (or TODO); the guarantee (or TODO).

---

## 5. 3-Year Picture  →  merge into `Context/strategy.md` `## 3-Year Picture` (`type: strategy`)

**Opening:** "Three years out — paint the picture. What's true about {company} then? Revenue,
headcount, what you sell, what it feels like."

**Follow-ups (one, if thin):**
- If purely qualitative: "Put a couple of numbers on it — revenue and team size in 3 years?"
- If purely numbers: "Beyond the numbers, what's different about how the company operates or feels?"

**Confirm-back:** "Your 3-year picture: {revenue}, {headcount}, {what's true}. Right?"

**Capture:** revenue, profit/headcount if given, and the qualitative "what's true" bullets.

---

## 6. 1-Year Plan  →  merge into `Context/strategy.md` `## 1-Year Plan` (`type: strategy`)

**Opening:** "This year — what does winning look like? Revenue and profit targets, and the 3 to 7
most important goals."

**Follow-ups (one, if thin):**
- If no measurables: "What 1 or 2 numbers will tell you mid-year whether you're on track?"
- If more than 7 goals: "Which 3 to 7 matter most? The plan works when it's focused."

**Confirm-back:** "Your 1-year plan: revenue {rev}, profit {profit}, measurables {metrics}, goals
{list}. Got it?"

**Capture:** revenue target, profit target, key measurables, the 3–7 goals.

---

## 7. Quarterly Rocks  →  `Context/rocks-{YYYY}-Q{n}.md` (`type: strategy`)

**Opening:** "Now this quarter — the next 90 days. What are the 3 to 7 most important things that
have to get done? We call these rocks."

**Follow-ups (one each, as needed):**
- No owner: "Who owns {rock}? One person accountable, not a team."
- Not measurable: "How will you know {rock} is *done* — what's the finish line?"
- More than 7: "Too many rocks and none get done. Which 3 to 7 are the real priorities?"

**Confirm-back:** "Your rocks for {quarter}: {rock → owner list}. Each with one owner and a clear
done. Right?"

**Capture:** each rock (specific statement) + its single owner + its done-criterion if given.
Each owner becomes a `[[wikilink]]` and should appear in the accountability chart.

---

## 8. Accountability Chart  →  `Context/accountability-chart.md` (`type: concept`) + `Team/{name}/{Name}.md` stubs (`type: person`)

**Opening:** "Last one — who's accountable for what. Start at the top: who's the Visionary (big
ideas, culture, relationships) and who's the Integrator (runs the day-to-day)?"

**Then walk the major seats:** "Now the major functions — sales/marketing, operations, finance, and
any others. For each, who's the one person accountable for it?"

**Follow-ups (one each, as needed):**
- Shared seat: "EOS rule: one name per seat. If two people share it, who's ultimately
  accountable?"
- A name you haven't met: "Tell me one line about {name} — their role in a sentence."

**Confirm-back:** "Your accountability chart: Visionary {name}, Integrator {name}, then {seat →
name list}. One name per seat. Accurate?"

**Capture:** each seat (function) + the one person accountable. Create a stub `person` page for any
named person not already in `Team/`.

---

## Handling "skip" / "not sure" on any section

Mark the section `deferred`. Acknowledge warmly ("No problem, I'll flag that to revisit"), record a
TODO for it in the relevant canon page (see canon-templates.md), and move to the next section. Never
fill a deferred answer with a guess or an EOS textbook default.

## Optional: Issues List

EOS's 8th V/TO box is the Issues List (open obstacles/ideas to solve). It's outside the core intake
scope (V/TO vision items + rocks + accountability chart). If the customer raises live issues during
the session, you may capture them at the end into `Intelligence/decisions/` or an issues note — but
do not add a question for it unless asked.
