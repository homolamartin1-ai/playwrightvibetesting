# Section 10 — Stability & Debugging

Make the suite trustworthy: kill flakiness, use traces/the debugger, and run the
regression pass against the fixed app.

## Course reference
| Prompt | Used in clip |
|--------|-------------|
| Prompt 1 — Audit the suite for flakiness | **Section 10, Clip 1** — Killing Flakiness |
| Prompt 2 — Enable traces and retries in config | **Section 10, Clip 1** — Killing Flakiness |
| Prompt 3 — Debug a failure from its trace | **Section 10, Clip 2** — Traces, Videos, and the UI Debugger |
| Prompt 4 — Regression pass on the fixed app | **Section 10, Clip 3** — Re-running Against the Fixed TechShop |

---

## Prompt 1: Audit the Suite for Flakiness
*Used in: Section 10, Clip 1 — "Killing Flakiness"*

```
Audit every spec in course-repo/tests for flakiness risks and fix what you find:

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
   cd course-repo/techshop/fixed-app && npx serve . -l 3000

Then run the full suite against it:
   npx playwright test

I expect the tests that failed against broken (the bug + regression cases) to now
PASS against fixed. Report any test that is STILL red — for each, tell me whether
the fix looks incomplete or our test is asserting the wrong thing.
```

**Expected:** a green (or near-green) run against the fixed app. Any remaining red
is real information about either the fix or the test.
