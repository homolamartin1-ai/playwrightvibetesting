# Section 9 — Bug Reports & Triage
**Section 9 · All clips**

A failing test is only **half** the job. Anyone can produce a red X. The value is
everything that happens next: deciding whether it is real, and communicating it so a
developer can act.

---

## Step 1 — Triage: real bug, or flaky test?

Before you report anything, answer this. Get it wrong one way and you cry wolf; get it
wrong the other and you ship a real defect.

The `flake-triage` skill works through this checklist:

| Question | Leans REAL BUG | Leans FLAKY TEST |
|---|---|---|
| **Consistency** — does it fail every run? | Fails every time | Fails intermittently |
| **Cause** — what does the failure point at? | The app behaving wrong | The test timing out, racing, or a fragile locator |
| **Evidence** — what does the trace show? | The app genuinely in a broken state | The test looking too early / at the wrong thing |

**If unsure, re-run it 3–5 times.** Consistency is the single biggest tell.

| Verdict | Action |
|---|---|
| **REAL BUG** | Write a bug report (below) |
| **FLAKY TEST** | Stabilise the test — do **not** file a bug. See [s10-debugging-and-flakiness.md](s10-debugging-and-flakiness.md) |

---

## Step 2 — The bug report template

```markdown
# BUG-004 — Cart discount divides by 1000 instead of 100, producing near-zero totals

**Environment:** TechShop broken-app · Chrome (latest) · http://localhost:3000
**Severity:** Critical
**Caught by:** `cart.spec.ts` → "line total reflects the discounted price"

## Steps to reproduce
1. Log in with `demo@techshop.com` / `password123`
2. Add "Laptop" ($100, 10% discount) to the cart
3. Open the cart

## Expected result
The line total shows **$90.00** (a 10% discount off $100).

## Actual result
The line total shows **$99.90** — the discount is applied as 10/1000 (1%) instead of
10/100 (10%).

## Severity justification
Critical — it corrupts the price of **every** discounted item in the store. Customers
would be charged the wrong amount.

## Requirement violated
"Cart shows: name, unit price, quantity, line total, order total"

## Evidence
- Playwright trace: `test-results/cart-line-total/trace.zip`
- Screenshot attached
```

---

## What makes each field matter

| Field | Get it wrong and… |
|---|---|
| **Title** | "Checkout broken" tells a developer nothing. Be **specific and behavioural**. |
| **Steps** | Vague steps → "cannot reproduce" → closed → the bug ships. |
| **Expected vs Actual** | Must be **separate fields**. Mashing them into one sentence hides the delta. |
| **Severity** | Always justify it. "Critical" with no reason gets ignored. |
| **Requirement** | Ties the bug to the spec — a product owner can act on it too. |
| **Evidence** | The trace is the proof. Attach it. |

---

## Severity guide

| Severity | Means | TechShop examples |
|---|---|---|
| **Critical** | Data corruption, security hole, or a core flow is impossible | Discount math wrong; checkout unreachable; empty form submits an order |
| **High** | A key feature is broken but has a workaround | Wrong password logs you in; cart total does not update |
| **Medium** | Feature works, but incorrectly or confusingly | CVV accepts letters; "Out of Stock" badge is green |
| **Low** | Cosmetic, no functional impact | Tab title says "Untitled" |

> **Severity vs Priority:** *Severity* is how bad it is. *Priority* is how soon it gets
> fixed. A Low-severity typo on the checkout button might be High priority if it is
> customer-facing.

---

## Step 3 — Review before you file

**The AI drafts; you sign off.** Your name is on the report.

- [ ] **Is it real?** You confirmed it — you did not just trust the red X.
- [ ] **Do the steps reproduce?** Could someone who was not here follow them exactly?
- [ ] **Is severity right?** Not over-rated, not under-rated — and justified.
- [ ] **Is the title specific?** Someone scanning a list knows immediately what is wrong.
- [ ] **Is the evidence attached?** Trace and/or screenshot.

> Every bad report erodes the trust you are building with your developers. Developers act
> on reports they can reproduce; they learn to ignore the rest.

---

## The classic failure this prevents

> A tester finds a genuine, critical bug. They write it up as *"checkout is broken"* with
> no steps. The developer cannot reproduce it. It gets closed as *cannot reproduce*. The
> bug ships. Users hit it.
>
> The tester **found** it. The communication failed.

That is the failure mode this whole section exists to eliminate.

---

## Where reports go

```
playwrightvibetesting/
└── bug-reports/
    ├── BUG-001-plaintext-password.md
    ├── BUG-004-discount-divides-by-1000.md
    └── …
```

Ready to paste into Jira or GitHub Issues.
