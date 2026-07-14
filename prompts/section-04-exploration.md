# Section 4 — Exploring TechShop with an AI Agent

> 📖 **Guide:** [Exploration Notes & Requirements Coverage](../docs/s04-exploration-notes.md) — the written companion for this section.

The agent drives the live browser via Playwright MCP. Nothing here writes test
files yet — this is discovery (the "exploration hat"). We work one flow at a
time: explore it, capture what we found, repeat for the next flow, then verify the
whole thing against the requirements.

## Course reference
| Prompt | Used in clip |
|--------|-------------|
| Prompt 1 — Explore the login flow | **Section 4, Clip 2** — The Agent Drives the Live App via MCP |
| Prompt 2 — Capture the login exploration notes | **Section 4, Clip 3** — Capturing Flows, Selectors, and Surprises |
| Prompt 3 — Repeat: explore & capture the remaining flows | **Section 4, Clip 3** — Capturing Flows, Selectors, and Surprises |
| Prompt 4 — Check coverage against the requirements | **Section 4, Clip 4** — Check Coverage Against the Requirements |

> TechShop (broken) must be running on http://localhost:3000.
> Requirements: `playwrightvibetesting/techshop/requirements.md`.

---

## Prompt 1: Explore the Login Flow
*Used in: Section 4, Clip 2 — "The Agent Drives the Live App via MCP"*

We start narrow — one flow — so you can watch the agent reason before scaling up.

```
Using the Playwright MCP browser on http://localhost:3000, walk through the
LOGIN flow only. For each step:
- describe what you did
- describe what you saw
- state what you expected to happen
- flag anything that does not match your expectation

Try a normal login (demo@techshop.com / password123), then try submitting the
form with empty fields, then try a wrong password. Watch the password field
specifically. Do not fix anything and do not write tests — just observe and report.
```

**Note:** the agent will surface real login issues — password shown in plaintext,
empty fields accepted, wrong credentials redirecting instead of erroring. It may
also raise false alarms; confirm the real ones yourself and dismiss the rest.

---

## Prompt 2: Capture the Login Exploration Notes
*Used in: Section 4, Clip 3 — "Capturing Flows, Selectors, and Surprises"*

Summarise **only what you just explored** — the login flow.

```
Based on the LOGIN exploration you just did, create a file at
playwrightvibetesting/exploration-notes.md with a "## Login" section containing
three parts:

### Flows
The exact steps to log in, as a numbered list.

### Selectors
For each element you interacted with (email field, password field, sign-in
button, error message), the recommended locator. Prefer role-based or
label-based locators (getByRole, getByLabel) over brittle CSS chains.

### Surprises
Every place login behaviour did not match expectation — what you did, what you
expected, what actually happened. These are login bug candidates for Section 7.

Keep it factual and concise. Cover login only for now — we add the other flows next.
```

---

## Prompt 3: Repeat — Explore & Capture the Remaining Flows
*Used in: Section 4, Clip 3 — "Capturing Flows, Selectors, and Surprises"*

Same explore-then-capture loop, now for the rest of the app.

```
Repeat the exploration-and-capture loop for each of these flows, one at a time,
using the Playwright MCP browser on http://localhost:3000:

1. Catalog — browse products, prices, descriptions, stock status
2. Cart — add a product, change quantity (including trying to go below 1), apply
   a discount, check whether the totals update
3. Checkout — fill the payment form, card validation, submit, confirmation page

For each flow: explore it the same way you explored login (what you did / saw /
expected / anything wrong), then APPEND a new "## Catalog", "## Cart", or
"## Checkout" section to playwrightvibetesting/exploration-notes.md with the same
Flows / Selectors / Surprises structure.

Pay attention to: cart math and quantity limits, the checkout button, card expiry
and CVV validation, the page title, and whether the navbar shows before login.
Still exploration only — do not write test files.
```

**Expected:** `exploration-notes.md` now covers all four flows — login (from
Prompt 2) plus catalog, cart, and checkout — the source material for the test
matrix in Section 7.

---

## Prompt 4: Check Coverage Against the Requirements
*Used in: Section 4, Clip 4 — "Check Coverage Against the Requirements"*

Hold the exploration up against the spec — cover the gaps, tie findings to rules.

```
Read two files:
- playwrightvibetesting/techshop/requirements.md — the spec of what TechShop should do
- playwrightvibetesting/exploration-notes.md — what we actually explored and found

Do two things and report back:

1. COVERAGE — go requirement by requirement. Did our exploration cover it? List
   every requirement we did NOT check yet. For each gap, explore it now using the
   Playwright MCP browser on http://localhost:3000 and record what you find.

2. COMPLIANCE — for every requirement we did check, state whether the app matches
   it (PASS) or deviates (FAIL). For each FAIL, quote the exact requirement it
   violates.

Then update playwrightvibetesting/exploration-notes.md: add any newly explored
flows, and tag each item under Surprises with the requirement it breaks. Finish
with a short coverage summary: requirements checked, passed, failed, and any that
are still untestable (for example, blocked by another bug).

Still exploration — do not write test files.
```

**Expected:** the notes now map findings to specific requirements, with a coverage
summary. That traceability is what makes the Section 7 test matrix complete and
defensible — every planned test traces back to a requirement.
