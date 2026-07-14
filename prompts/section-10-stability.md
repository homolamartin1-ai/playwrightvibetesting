# Section 10 — Stability & Debugging

> 📖 **Guide:** [Debugging & Killing Flakiness](../docs/s10-debugging-and-flakiness.md) — the written companion for this section.

Make the suite trustworthy: kill flakiness, use traces/the debugger, and run the
regression pass against the fixed app.

## Course reference
| Prompt | Used in clip |
|--------|-------------|
| Prompt 1 — Audit the suite for flakiness | **Section 10, Clip 1** — Killing Flakiness |
| Prompt 2 — Enable traces and retries in config | **Section 10, Clip 1** — Killing Flakiness |
| Prompt 3 — Debug a failure from its trace | **Section 10, Clip 2** — Traces, Videos, and the UI Debugger |
| Prompt 4 — Regression pass on the fixed app | **Section 10, Clip 3** — Re-running Against the Fixed TechShop |
| Prompt 5 — Implement the remaining fixes and re-verify | **Section 10, Clip 4** — Closing the Loop: Fix, Then Re-verify |

---

## Prompt 1: Audit the Suite for Flakiness
*Used in: Section 10, Clip 1 — "Killing Flakiness"*

```
Audit every spec in playwrightvibetesting/tests for flakiness risks and fix what you find:

- Manual waits / sleeps (page.waitForTimeout, fixed delays) → remove and rely on
  Playwright auto-waiting and web-first assertions
- Fragile locators (CSS/structural chains) → replace with getByRole / getByLabel
- Tests that depend on another test's leftover state → make each test set up its
  own state so it can run in isolation, in any order

Report each change you make and why. Do not weaken any assertion to make a test
pass — stability must not cost correctness.
```

---

## Prompt 2: Enable Traces and Retries in Config
*Used in: Section 10, Clip 1 — "Killing Flakiness"*

```
Update playwright.config.ts:
- record a trace on first retry (trace: 'on-first-retry')
- record video for failures (video: 'retain-on-failure')
- set retries to 2 ONLY when running in CI (process.env.CI ? 2 : 0)
- set baseURL to http://localhost:3000 so specs can use relative paths

Explain why retries are a CI safety net for rare noise, not a fix for a genuinely
flaky test.
```

---

## Prompt 3: Debug a Failure From Its Trace
*Used in: Section 10, Clip 2 — "Traces, Videos, and the UI Debugger"*

```
Run the suite so a trace is produced, then open the trace viewer:
   npx playwright test
   npx playwright show-trace

For [the failing test], read the trace and tell me, step by step, what the page
looked like at the moment it failed, where the test's expectation diverged from
reality, and whether this is the app being wrong or the test being wrong. If it
is the test, propose the fix.
```

> Also useful: `npx playwright test --ui` opens the interactive UI mode to
> time-travel through actions and watch locators highlight.

---

## Prompt 4: Regression Pass on the Fixed App
*Used in: Section 10, Clip 3 — "Re-running Against the Fixed TechShop"*

First serve the fixed app instead of the broken one:

```
Stop the broken-app server. Serve the FIXED app on the same port so the suite
points at it unchanged:
   cd playwrightvibetesting/techshop/fixed-app && npx serve . -l 3000

Then run the full suite against it:
   npx playwright test

I expect the tests that failed against broken (the bug + regression cases) to now
PASS against fixed. Report any test that is STILL red — for each, tell me whether
the fix looks incomplete or our test is asserting the wrong thing.
```

---

## Prompt 5: Implement the Remaining Fixes and Re-verify
*Used in: Section 10, Clip 4 — "Closing the Loop: Fix, Then Re-verify"*

Close out the tests still red against the fixed app — because the app is genuinely
still wrong, not the test. Triage first (Section 9 flake-triage), then fix and rerun.

```
Some tests are still failing against the FIXED TechShop app. For each one that
triage confirmed is a REAL bug (not a flaky test):

1. Trace the failure to the responsible code in
   playwrightvibetesting/techshop/fixed-app (index.html / app.js / style.css).
2. Explain the root cause, and propose the SMALLEST change that makes the app meet
   the matching rule in playwrightvibetesting/techshop/requirements.md.
3. Show me the proposed diff and WAIT for my approval before changing anything.
4. After I approve, apply the change to the fixed app.

When all approved fixes are in, re-run the whole suite against the fixed app:
   npx playwright test

Report the result. Do NOT weaken, skip, or delete any test to force a pass — the
app must actually satisfy the test. We are done only when the run is fully green
because the app is correct.
```

**Expected:** the still-red tests turn green because the fixed app was corrected —
not because the tests were loosened. A fully green run against fixed-app means
verification is genuinely complete: find → fix → re-verify, with you approving
every change that lands.
