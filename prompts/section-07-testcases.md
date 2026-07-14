# Section 7 — From Exploration to Test Cases

> 📖 **Guide:** [Test Case & Matrix Format](../docs/s07-test-case-format.md) — the written companion for this section.

Apply the test-case design skill (Section 6) to the exploration notes (Section 4)
to produce a complete, prioritised test matrix.

## Course reference
| Prompt | Used in clip |
|--------|-------------|
| Prompt 1 — Design the full test matrix | **Section 7, Clip 1** — Turning Exploration Notes into a Test Matrix |
| Prompt 2 — Review for category gaps | **Section 7, Clip 2** — Positive, Negative, and Edge Cases |
| Prompt 3 — Add planned regression cases | **Section 7, Clip 3** — Converting Known Bugs into Planned Regressions |

---

## Prompt 1: Design the Full Test Matrix
*Used in: Section 7, Clip 1 — "Turning Exploration Notes into a Test Matrix"*

```
Following playwrightvibetesting/skills/test-case-design.md, and using
playwrightvibetesting/exploration-notes.md as the source of truth for how TechShop behaves,
design the complete test matrix for these features:

- Login
- Catalog / product listing
- Cart (quantities, discount, totals)
- Checkout (card validation, submission, confirmation)

Produce positive, negative, and edge cases for each, in the skill's format.
Save the result to playwrightvibetesting/test-cases.md, grouped by feature.
```

---

## Prompt 2: Review for Category Gaps
*Used in: Section 7, Clip 2 — "Positive, Negative, and Edge Cases"*

You are the editor — make the agent prove its coverage.

```
Review playwrightvibetesting/test-cases.md. For each feature, check that all three
categories are genuinely covered:

- Did any feature get only happy-path (positive) cases? Add the missing
  negative and edge cases.
- Are there negative cases specific to TechShop that a generic template would
  miss (e.g. card expiry in the past, CVV with letters, quantity below 1)?
- Call out any feature where coverage still feels thin and propose the cases
  needed to close the gap.

Update the file with anything missing.
```

---

## Prompt 3: Add Planned Regression Cases
*Used in: Section 7, Clip 3 — "Converting Known Bugs into Planned Regressions"*

```
We already know TechShop has these bugs (confirmed in exploration):

- Password field shows text in plaintext (should be masked)
- Empty email/password is accepted (should be rejected)
- Wrong credentials redirect to the catalog (should show an error)
- Cart discount divides by 1000 instead of 100 (totals are wrong)
- Cart allows quantities below 1 (negative quantities)
- Order total does not update when quantity changes
- Checkout accepts a past card expiry date
- CVV accepts letters and symbols
- "Proceed to Checkout" button does not respond (blocker)
- Checkout form submits with all fields empty
- Confirmation page is missing the order reference number
- Browser tab title shows "Untitled"
- Navbar is visible on the login page before authentication

For each, add a REGRESSION test case to playwrightvibetesting/test-cases.md that asserts the
CORRECT behaviour. These should FAIL against the broken app and PASS against the
fixed app. Mark each clearly as a regression case and note the related bug.

For the "Proceed to Checkout" blocker, note that it blocks the checkout flow and
that those tests will stay red until the blocker is fixed.
```

**Expected:** `test-cases.md` now contains positive, negative, edge, and
regression cases — the blueprint for the suite we write in Section 8.
