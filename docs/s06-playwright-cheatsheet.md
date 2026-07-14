# Section 6 — Playwright Cheat Sheet
**Section 6 · All clips** · Your reference for the rest of the course

You do not need to write Playwright from scratch. You need to **read what the agent
writes and know whether it is any good**. This page is what you come back to.

---

## Locators — how Playwright finds things

### ✅ Good — reads like a user thinks

```ts
page.getByRole('button', { name: 'Sign In' })   // by role + accessible name
page.getByLabel('Password')                      // by form label
page.getByPlaceholder('Enter username')          // by placeholder
page.getByText('Your cart is empty')             // by visible text
page.getByTestId('cart-total')                   // by data-testid (stable by design)
```

### ❌ Fragile — breaks on the next redesign

```ts
page.locator('div.container > div:nth-child(3) > span.price')  // structural chain
page.locator('#app > div > div > button')                       // position-dependent
```

> **Review rule #1:** open any AI-generated test and look at the locators first. Readable
> and role-based? Good. Long CSS chains? Flag it.

### Useful modifiers
```ts
page.getByRole('listitem').filter({ hasText: 'Laptop' })  // narrow a list
page.getByRole('button', { name: 'Remove' }).first()      // when several match
page.locator('.product-card').nth(2)                       // by index
```

---

## Assertions — where the test decides pass/fail

Always `await` them. These are **web-first assertions** — they auto-retry until they pass
or time out.

```ts
await expect(page.getByText('Welcome')).toBeVisible();
await expect(page.getByRole('button', { name: 'Pay' })).toBeEnabled();
await expect(page.getByTestId('total')).toHaveText('$90.00');
await expect(page.getByLabel('Password')).toHaveAttribute('type', 'password');
await expect(page).toHaveTitle('TechShop — Products');
await expect(page).toHaveURL(/products/);
await expect(page.getByRole('listitem')).toHaveCount(3);
```

### ✅ Meaningful vs ❌ weak

```ts
// ❌ weak — passes even if the feature is broken
await expect(page).toBeTruthy();
await expect(addButton).toBeVisible();          // the button existing proves nothing

// ✅ meaningful — checks the actual outcome
await addButton.click();
await expect(page.getByTestId('cart-count')).toHaveText('1');
await expect(page.getByTestId('cart-total')).toHaveText('$90.00');
```

> **Review rule #2:** for every assertion, ask *"would this still pass if the feature were
> broken?"* If yes, it is a **false green**.

---

## Auto-waiting — why there are no sleeps

Before clicking or asserting, Playwright automatically waits for the element to be
attached, visible, stable, and enabled.

```ts
// ✅ correct — Playwright waits for you
await page.getByRole('button', { name: 'Sign In' }).click();

// ❌ never do this
await page.waitForTimeout(2000);   // slow AND unreliable
```

> **Review rule #3:** if AI-generated code contains `waitForTimeout`, delete it. It is a
> smell, and it is the #1 cause of flaky suites.

Legitimate explicit waits (rare):
```ts
await page.waitForURL('**/checkout');
await expect(spinner).toBeHidden();   // wait for something to disappear
```

---

## Anatomy of a test

```ts
import { test, expect } from '@playwright/test';

test('login rejects an empty password', async ({ page }) => {
  await page.goto('/');                                  // navigate
  await page.getByLabel('Username').fill('demo@techshop.com');
  await page.getByRole('button', { name: 'Sign In' }).click();   // interact
  await expect(page.getByText('Password is required')).toBeVisible();  // assert
});
```

**Navigate → interact → assert.** That is the rhythm of nearly every test you will read.

---

## Project structure

```
tests/
├── fixtures.ts          ← shared setup (login)
├── login.spec.ts        ← one spec per feature area
├── cart.spec.ts
└── checkout.spec.ts
playwright.config.ts     ← the control panel
```

---

## `playwright.config.ts` — the settings that matter

```ts
export default defineConfig({
  testDir: './tests',
  use: {
    baseURL: 'http://localhost:3000',     // lets tests use page.goto('/')
    trace: 'on-first-retry',              // record a trace when a test retries
    video: 'retain-on-failure',           // keep video only for failures
  },
  retries: process.env.CI ? 2 : 0,        // retries in CI only
  reporter: 'html',
  projects: [ { name: 'chromium', use: { ...devices['Desktop Chrome'] } } ],
});
```

---

## CLI commands you will actually use

```bash
npx playwright test                      # run everything
npx playwright test login.spec.ts        # run one file
npx playwright test -g "empty password"  # run tests matching a name
npx playwright test --headed             # watch the browser
npx playwright test --ui                 # interactive UI mode (best for debugging)
npx playwright show-report               # open the HTML report
npx playwright show-trace                # open a trace file
npx playwright install                   # (re)install browsers
```

---

## The three-question review checklist

Run this on **every** test the agent writes:

1. **Locators** — readable and role-based, or a fragile CSS chain?
2. **Assertions** — do they check the real outcome, or just that the page survived?
3. **False greens** — would this pass even if the feature were broken?

And the proof: **break the app on purpose and confirm the test goes red.**

> A test that has never failed is a test you cannot trust yet.
