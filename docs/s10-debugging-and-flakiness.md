# Section 10 — Debugging & Killing Flakiness
**Section 10 · All clips**

A test suite has one job beyond catching bugs: **it has to be trusted.** A suite nobody
trusts gets switched off — and then a real failure looks exactly like the noise everyone
learned to ignore.

---

## Part 1 — Killing flakiness

A **flaky test** passes sometimes and fails other times without anything changing.

### The three causes, and their fixes

| # | Cause | Fix |
|---|---|---|
| 1 | **Timing** — the test acts before the app is ready | Let Playwright auto-wait. **Delete every `waitForTimeout`.** |
| 2 | **Fragile locators** — tied to page structure | Replace with `getByRole` / `getByLabel`. |
| 3 | **Shared state** — test B depends on test A | Make every test set up its own state. |

```ts
// ❌ flaky
await page.waitForTimeout(2000);
await expect(page.locator('div > span.total')).toHaveText('$90.00');

// ✅ stable — web-first assertion auto-retries until it passes or times out
await expect(page.getByTestId('cart-total')).toHaveText('$90.00');
```

### Retries — a safety net, not a cure

```ts
retries: process.env.CI ? 2 : 0,   // retries in CI only
```

Retries absorb rare infrastructure noise in CI. They do **not** fix a flaky test.

> ⚠️ **If a test only passes on retry, something is wrong with it.** Do not paper over it.

### Config that helps you debug

```ts
use: {
  trace: 'on-first-retry',       // record a trace when a test retries
  video: 'retain-on-failure',    // keep video only for failures
  screenshot: 'only-on-failure',
}
```

---

## Part 2 — The three debugging tools

### 1. Traces (the most useful)

A trace is a **complete recording** of everything the test did — every action, every
navigation, and a DOM snapshot at each step. You scrub through it like a video with a
timeline.

```bash
npx playwright test                 # produces traces per your config
npx playwright show-trace           # pick a trace file to open
npx playwright show-trace test-results/<test-name>/trace.zip
```

For a confusing failure, the trace usually shows the answer in seconds:
*"oh — the page was still on the login screen, the click never landed."*

### 2. Video

```ts
video: 'retain-on-failure'
```
Sometimes just watching the run reveals it instantly — the modal that popped up and
blocked the button.

### 3. UI Mode (best for developing tests)

```bash
npx playwright test --ui
```

An interactive window: run tests, watch them execute step by step, see locators highlight
on the page, and time-travel through each action. This is where you will spend time when
something stubborn refuses to cooperate.

Also useful:
```bash
npx playwright test --headed        # just watch the browser
npx playwright test --debug          # step through with the inspector
```

---

## Part 3 — Hand the evidence to the agent

When a failure is confusing, do not stare at it. Paste the evidence into the chat:

```
This test is failing and I do not understand why. Here is the error and the trace.
Read the trace, tell me step by step what the page looked like when it failed, where
the test's expectation diverged from reality, and whether this is the APP being wrong
or the TEST being wrong. If it is the test, propose the fix.
```

You still judge whether it is right — but it accelerates the diagnosis enormously.

---

## Part 4 — The regression pass, and closing the loop

### Point the suite at the fixed app

```bash
# stop the broken-app server first
cd playwrightvibetesting/techshop/fixed-app
npx serve . -l 3000        # or: python3 -m http.server 3000

npx playwright test
```

The tests that proved the app was broken should now prove it is **fixed**.

### When some tests are *still* red

A fixed release rarely comes back 100% green on the first pass. That is not the suite
failing — that is the suite **doing its job**.

For each still-red test:

1. **Triage it** (real bug vs flaky — see [s09-bug-report-guide.md](s09-bug-report-guide.md))
2. If it is a **real bug**, the fix was incomplete. Use **Prompt 5** in
   [`prompts/section-10-stability.md`](../prompts/section-10-stability.md) — the agent
   traces the failure to the code in `fixed-app`, proposes the smallest change that meets
   the requirement, and **shows you the diff and waits for your approval**.
3. Apply the approved fix, then **re-run until green**.

### 🚫 The rule that holds it all together

> **Never weaken, skip, or delete a test to force a pass.**
> The app must satisfy the test — not the other way around.

That is the loop: **find → fix → re-verify**, driven by the suite, with you approving
every change that lands.

---

## The standard to hold yourself to

A test is only allowed in your suite if you trust it **both ways**:

- **Green** means working.
- **Red** means broken.
- No *maybe*. No *re-run and hope*.

Run the whole suite three times. Same result each time? You have a trustworthy suite.
