# Section 9 — Finding & Reporting Bugs

Run the full suite, triage failures (real bug vs. flaky) with the flake-triage
skill, and turn confirmed bugs into reports with the bug-reporting skill.

## Course reference
| Prompt | Used in clip |
|--------|-------------|
| Prompt 1 — Run the full suite + HTML report | **Section 9, Clip 1** — Running the Suite and Reading Failures |
| Prompt 2 — Triage a failure | **Section 9, Clip 2** — App Bug or Test Bug? Flake-Triage in Action |
| Prompt 3 — Introduce a flaky failure (demo) | **Section 9, Clip 2** — App Bug or Test Bug? |
| Prompt 4 — Generate a bug report | **Section 9, Clip 3** — The Bug-Reporting Skill in Action |
| Prompt 5 — Batch the rest of the reports | **Section 9, Clip 3** — The Bug-Reporting Skill in Action |

---

## Prompt 1: Run the Full Suite + HTML Report
*Used in: Section 9, Clip 1 — "Running the Suite and Reading Failures"*

```
Run the entire Playwright suite against the broken TechShop and then open the
HTML report:
   npx playwright test
   npx playwright show-report

Summarise: how many passed, how many failed, and for each failure give me the
test name and the one-line reason (expected vs actual). Do not fix anything yet.
```

---

## Prompt 2: Triage a Failure
*Used in: Section 9, Clip 2 — "App Bug or Test Bug?"*

```
Following course-repo/skills/flake-triage.md, triage this failing test:
[paste the test name and its error, or point to it in the report].

Work through the checklist (consistency, cause, evidence) and classify it as
REAL BUG or FLAKY TEST, with your reasoning and a recommendation. If consistency
is unclear, re-run the test a few times before deciding.
```

---

## Prompt 3: Introduce a Flaky Failure (Teaching Demo)
*Used in: Section 9, Clip 2 — "App Bug or Test Bug?"*

So students see the triage skill catch a *non*-bug.

```
To demonstrate triage, deliberately make ONE test flaky in a temporary copy —
for example add a fixed 100ms wait and assert against an element before it could
render, or use a deliberately fragile locator. Run it several times so it passes
sometimes and fails sometimes. Then run it through flake-triage.md and confirm
the skill classifies it as a FLAKY TEST, not a real bug. Revert the change after.
```

**Expected:** the skill distinguishes the intermittent, test-caused failure from
the consistent, app-caused ones. That distinction is the whole point.

---

## Prompt 4: Generate a Bug Report
*Used in: Section 9, Clip 3 — "The Bug-Reporting Skill in Action"*

```
Following course-repo/skills/bug-reporting.md, turn this confirmed failure into a
bug report:
[the discount-calculation test failure].

Use the failing test and its Playwright trace as the source. Output the report in
the skill's format (title, environment, steps, expected, actual, severity +
justification, evidence). Save it to course-repo/bug-reports/BUG-discount.md.
```

**Expected:** a specific title, separated expected/actual, justified severity, and
the trace referenced — a report a developer can act on without talking to you.

---

## Prompt 5: Batch the Rest of the Reports
*Used in: Section 9, Clip 3 — "The Bug-Reporting Skill in Action"*

```
Following bug-reporting.md, generate a report for each remaining CONFIRMED real
bug from the suite run (skip anything triaged as flaky). Save each to
course-repo/bug-reports/ as its own file.

Then give me a summary table: bug ID, title, severity, and the test that caught
it. I will review every report before any of them get filed.
```

> Review gate (Clip 4): before filing, confirm each bug is real, the steps
> reproduce, severity is right, and the title is specific. The AI drafts; you
> sign off.
