# Section 5 — Skills for AI Agents: Building the Toolkit

The centerpiece. Each prompt creates one reusable skill file in
`playwrightvibetesting/skills/`, then a follow-up prompt proves the skill works. These
skills are reused (and refined) for the rest of the course — and you keep them.

## Course reference
| Prompt | Used in clip |
|--------|-------------|
| Prompt 1 — Build the test-case design skill | **Section 5, Clip 2** — Skill 1: Test-Case Design |
| Prompt 1b — Try it | **Section 5, Clip 2** |
| Prompt 2 — Build the test-authoring skill | **Section 5, Clip 3** — Skill 2: Test Authoring |
| Prompt 2b — Try it | **Section 5, Clip 3** |
| Prompt 3 — Build the bug-reporting skill | **Section 5, Clip 4** — Skill 3: Bug Reporting |
| Prompt 4 — Build the flake-triage skill | **Section 5, Clip 5** — Skill 4: Flake Triage |

> A "skill" here is a markdown file the agent reads before doing a task, so it
> applies your standards every time instead of improvising per prompt. In
> Antigravity, save these to `playwrightvibetesting/skills/` and reference them in prompts.

---

## Prompt 1: Build the Test-Case Design Skill
*Used in: Section 5, Clip 2 — "Skill 1: Test-Case Design"*

```
Create a reusable skill file at playwrightvibetesting/skills/test-case-design.md.

Its purpose: given a feature and exploration notes, produce a complete test
matrix. Write it as instructions YOU (the agent) will follow whenever I ask you
to design test cases. It must require:

- THREE categories for every feature:
  - Positive — the feature works as intended with valid input
  - Negative — invalid input is rejected (empty fields, wrong values, bad formats)
  - Edge — boundaries (min/max, zero, just-below-minimum, past dates, max length)
- A consistent format per case: ID, title, preconditions, steps, expected result
- Titles that describe behaviour, e.g. "Login rejects empty password"
- Coverage tied to the real app: use the exploration notes, not generic guesses
- A reminder to flag any known bug as a planned regression case

Keep it concise and instructional. This file is the standard, not an example.
```

## Prompt 1b: Try the Test-Case Design Skill
*Used in: Section 5, Clip 2*

```
Following playwrightvibetesting/skills/test-case-design.md exactly, and using
playwrightvibetesting/exploration-notes.md, design the test cases for the LOGIN feature
only. Output the matrix so I can review it.
```

**Expected:** a structured matrix (positive / negative / edge) in the skill's
format. If it drifts from the format, fix the skill, not the output.

---

## Prompt 2: Build the Test-Authoring Skill
*Used in: Section 5, Clip 3 — "Skill 2: Test Authoring"*

```
Create a reusable skill file at playwrightvibetesting/skills/test-authoring.md.

Its purpose: turn test cases into Playwright tests to a consistent standard.
Write it as instructions you will follow whenever I ask you to write tests. It
must require:

- LOCATORS: prefer getByRole and getByLabel; never use brittle CSS/structural
  chains; if no good locator exists, recommend adding a stable one
- ASSERTIONS: every test must assert the real outcome (state, value, visibility),
  never just "the page did not crash"
- STRUCTURE: one spec file per feature area (login, cart, checkout); use a light
  Page Object for each area to hold locators and actions
- FIXTURES: shared setup (like login) goes in a fixture, never copy-pasted
- ISOLATION: every test runs independently, in any order, with its own setup
- SECRETS: credentials come from env vars (TEST_EMAIL / TEST_PASSWORD); never
  hardcode them
- FORBIDDEN: no manual sleeps/waits (rely on auto-waiting), no hardcoded secrets,
  no assertions on values that never change

Keep it concise. This is the rulebook the suite is built on.
```

## Prompt 2b: Try the Test-Authoring Skill
*Used in: Section 5, Clip 3*

```
Following playwrightvibetesting/skills/test-authoring.md, write the login spec from the
test cases we designed. Put it at playwrightvibetesting/tests/login.spec.ts. Then walk me
through how it satisfies each rule in the skill.
```

**Expected:** clean role/label locators, real assertions, a login fixture, no
hardcoded secrets. Review it with the Section 5 checklist before trusting it.

---

## Prompt 3: Build the Bug-Reporting Skill
*Used in: Section 5, Clip 4 — "Skill 3: Bug Reporting"*

```
Create a reusable skill file at playwrightvibetesting/skills/bug-reporting.md.

Its purpose: turn a failing test + its trace into a developer-ready bug report.
Write it as instructions you will follow whenever I ask you to report a bug. It
must require this structure:

- Title: specific and behavioural (not "checkout broken" but
  "Proceed to Checkout button does not respond to click")
- Environment: app version (broken/fixed), browser, URL
- Steps to reproduce: numbered, exact, reproducible by someone who was not here
- Expected result and Actual result: kept as SEPARATE fields, never one sentence
- Severity: Critical / High / Medium / Low, WITH a one-line justification
- Evidence: reference the Playwright trace and any screenshot

Also require a final self-check: "Could a developer reproduce this from the steps
alone?" If not, the report is not done.

Keep it concise and reusable across any project.
```

**Note:** we put this skill to work in Section 9. For now, build it and (if you
have a failing test already) feed one failure through it as a quick test.

---

## Prompt 4: Build the Flake-Triage Skill
*Used in: Section 5, Clip 5 — "Skill 4: Flake Triage"*

```
Create a reusable skill file at playwrightvibetesting/skills/flake-triage.md.

Its purpose: decide whether a failing test is a REAL BUG or a FLAKY TEST. Write
it as instructions you will follow whenever I give you a failure. It must require
working through this checklist before concluding:

1. Consistency: does it fail every run, or only sometimes? (Consistent → leans
   real bug. Intermittent → leans flaky.)
2. Cause: does the failure show the APP behaving wrong, or the TEST timing out,
   racing, or using a fragile locator?
3. Evidence: in the trace, was the app genuinely in a broken state, or was the
   test just confused/looking too early?
4. Classification: state clearly "REAL BUG" or "FLAKY TEST".
5. Recommendation: if real bug → use the bug-reporting skill. If flaky → name the
   specific fix (better locator, remove sleep, fix isolation), do not file a bug.

Require the agent to ask to re-run a test a few times when consistency is unclear.
Keep it concise.
```

**Expected:** four skill files now live in `playwrightvibetesting/skills/`. They are the
toolkit for Sections 7–12 — and the thing you carry to your next project.
