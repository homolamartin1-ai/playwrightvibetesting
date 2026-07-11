# Section 12 — Capstone Project: BookNow

This is the independent project. By design, there are **no ready-made prompts
here** — writing them is part of the exercise. Instead, this file maps each
capstone task to the prompt file you adapt from the main course.

The app: `playwrightvibetesting/capstone/booknow-broken` (and `booknow-fixed` for the
verification pass). Serve it the same way as TechShop:

```
cd playwrightvibetesting/capstone/booknow-broken && npx serve . -l 3000
```

BookNow is a hotel booking app — login, search results, booking form,
confirmation. Different domain, different rules (dates, check-in before
check-out, guest counts, availability). Adapt your prompts; do not copy TechShop
specifics blindly.

The spec is in `playwrightvibetesting/capstone/requirements.md` — use it to check
coverage and tie every finding to a requirement, exactly like Section 4.

---

## Task → prompt to adapt

| Capstone task | Adapt from | What changes for BookNow |
|---------------|-----------|--------------------------|
| 1. Explore with the agent | `section-04-exploration.md` (Prompt 1–3) | New flows: login → search → booking form → confirmation. Watch the date logic. |
| 1b. Check coverage vs the spec | `section-04-exploration.md` (Prompt 4) | Read `capstone/requirements.md`; explore any uncovered rule and tag each finding to the requirement it breaks. |
| 2. Design test cases | `section-07-testcases.md` (Prompt 1–2) + `skills/test-case-design.md` | Add booking-specific edges: check-in after check-out, zero guests, past dates. |
| 3. Write the suite | `section-08-writing-suite.md` + `skills/test-authoring.md` | Page objects for the booking flow; refine the skill if the new domain exposes a gap. |
| 4. Run, triage, report | `section-09-bugs.md` + `skills/flake-triage.md` + `skills/bug-reporting.md` | Same loop; you decide real-bug vs flaky and write the reports. |
| 5. Verify | `section-10-stability.md` (Prompt 4) | Point the suite at `booknow-fixed`; failing cases should flip to green. |
| 6. Ship to CI | `section-11-ci.md` (Prompt 1–2) | Same workflow, serving the BookNow app. |

---

## The one rule

Reuse your **skills** — do not start from zero. The four files in
`playwrightvibetesting/skills/` are the whole point: you begin the capstone from leverage,
exactly like you will begin your next real project. If a skill shows a gap on
this new domain, **refine the skill**, then continue.

## Self-check before you call it done
- Coverage tells the story of the whole app (positive / negative / edge per feature)
- Every failing test fails for the right reason (break the app on purpose to confirm)
- Every bug report is reproducible by someone who was not there
- The suite gives the same result on three consecutive runs (no flakiness)
- CI is green against the fixed app, meaningful red against the broken one

> A walkthrough of a completed BookNow capstone is in the course resources. Watch
> it **after** your own attempt, not before.
