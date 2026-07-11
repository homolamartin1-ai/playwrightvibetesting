# Section 4 — Exploring TechShop with an AI Agent

The agent drives the live browser via Playwright MCP. Nothing here writes test
files yet — this is discovery (the "exploration hat").

## Course reference
| Prompt | Used in clip |
|--------|-------------|
| Prompt 1 — Explore a single flow | **Section 4, Clip 2** — The Agent Drives the Live App via MCP |
| Prompt 2 — Full guided exploration | **Section 4, Clip 2** — The Agent Drives the Live App via MCP |
| Prompt 3 — Capture exploration notes | **Section 4, Clip 3** — Capturing Flows, Selectors, and Surprises |

> TechShop (broken) must be running on http://localhost:3000.

---

## Prompt 1: Explore a Single Flow
*Used in: Section 4, Clip 2 — "The Agent Drives the Live App via MCP"*

Start narrow so you can see the agent's reasoning before turning it loose.

```
Using the Playwright MCP browser on http://localhost:3000, walk through the
LOGIN flow only. For each step:
- describe what you did
- describe what you saw
- state what you expected to happen
- flag anything that does not match your expectation

Try a normal login (demo@techshop.com / password123), then try submitting the
form with empty fields, then try a wrong password. Do not fix anything and do
not write tests — just observe and report.
```

---

## Prompt 2: Full Guided Exploration
*Used in: Section 4, Clip 2 — "The Agent Drives the Live App via MCP"*

```
Using the Playwright MCP browser on http://localhost:3000, explore the full
TechShop user journey end to end:

1. Login
2. Browse the product catalog
3. Add a product to the cart
4. Open the cart and change the quantity (including trying to go below 1)
5. Apply any discount and check the totals
6. Proceed to checkout and fill the payment form
7. Reach the confirmation page

At each step, report what you did, what you saw, what you expected, and anything
that looks like a bug. Pay attention to: the password field, cart math, quantity
limits, the checkout button, card validation, the page title, and whether the
navbar appears before login.

This is exploration only — do not write test files.
```

**Note:** the agent will surface real issues (plaintext password, negative
quantities, an unresponsive "Proceed to Checkout" button, etc.). It may also raise
false alarms — confirm the real ones yourself, dismiss the rest, and steer it to
anything it skipped. You are the senior tester guiding a fast junior.

---

## Prompt 3: Capture Exploration Notes
*Used in: Section 4, Clip 3 — "Capturing Flows, Selectors, and Surprises"*

```
Based on the exploration you just did on TechShop, create a file at
course-repo/exploration-notes.md with three sections:

## Flows
For each area (login, catalog, cart, checkout), the exact steps to move through
it, written as a numbered list.

## Selectors
For each key element you interacted with, how to locate it. Prefer role-based or
label-based locators (e.g. getByRole, getByLabel) over brittle CSS chains. List
the element name and the recommended locator.

## Surprises
Every place where actual behaviour did not match expected behaviour. For each:
what you did, what you expected, what actually happened. These are our bug
candidates for Section 7.

Keep it factual and concise — this file is the source material for designing
test cases and writing the suite.
```

**Expected output:** a structured `exploration-notes.md` we reuse in Sections 7
and 8. The quality of this file determines the quality of the suite.
