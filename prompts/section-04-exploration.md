# Section 4 — Exploring TechShop with an AI Agent

The agent drives the live browser via Playwright MCP. Nothing here writes test
files yet — this is discovery (the "exploration hat"). We work one flow at a
time: explore it, capture what we found, then repeat for the next flow.

## Course reference
| Prompt | Used in clip |
|--------|-------------|
| Prompt 1 — Explore the login flow | **Section 4, Clip 2** — The Agent Drives the Live App via MCP |
| Prompt 2 — Capture the login exploration notes | **Section 4, Clip 3** — Capturing Flows, Selectors, and Surprises |
| Prompt 3 — Repeat: explore & capture the remaining flows | **Section 4, Clip 3** — Capturing Flows, Selectors, and Surprises |

> TechShop (broken) must be running on http://localhost:3000.

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
