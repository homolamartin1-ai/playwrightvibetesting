# Section 8 — Suite Structure, Fixtures & Credentials
**Section 8 · Clips 1–2** (and everything after)

Three things every real suite has to solve: **authentication**, **test data**, and
**isolation**. Get them right and the suite is fast, secure, and reliable. Get them wrong
and it is slow, leaky, and flaky.

---

## 🔐 Credentials — set this up before you write any test

**Never hardcode credentials in a spec file.** They come from environment variables.

### 1. Create `.env` from the example

```bash
cd playwrightvibetesting
cp .env.example .env
```

### 2. Fill it in

```bash
# .env
TEST_EMAIL=demo@techshop.com
TEST_PASSWORD=password123
```

### 3. Confirm `.env` is gitignored

```bash
git check-ignore .env    # prints ".env" → good, it will never be committed
```

> `.env.example` **is** committed (so others know what to provide).
> `.env` **is not** (it holds the real values).

### 4. Load it in `playwright.config.ts`

```ts
import { defineConfig } from '@playwright/test';
import dotenv from 'dotenv';

dotenv.config();   // reads .env into process.env

export default defineConfig({ /* … */ });
```

Install the loader if needed: `npm install -D dotenv`

### 5. Use it in tests

```ts
const email = process.env.TEST_EMAIL!;
const password = process.env.TEST_PASSWORD!;
```

> ⚠️ In **CI** there is no `.env` file — the values come from **GitHub repository
> secrets** instead. See [s11-github-actions-ci.md](s11-github-actions-ci.md).

---

## Page Objects — or not?

A **Page Object** puts a page's locators and actions in one class, so when the page
changes you update *one place* instead of twenty test files.

**The honest answer: it depends on size and lifespan.**

| Suite | Verdict |
|---|---|
| A handful of tests on a stable app | POM is overkill — keep it simple |
| A growing suite you maintain for years | POM earns its keep |

For TechShop we use a **light POM** — enough to keep locators in one place and tests
readable.

```ts
// tests/pages/LoginPage.ts
import { Page } from '@playwright/test';

export class LoginPage {
  constructor(private page: Page) {}

  private username = () => this.page.getByLabel('Username');
  private password = () => this.page.getByLabel('Password');
  private signIn   = () => this.page.getByRole('button', { name: 'Sign In' });

  async goto() {
    await this.page.goto('/');
  }

  async login(email: string, pass: string) {
    await this.username().fill(email);
    await this.password().fill(pass);
    await this.signIn().click();
  }
}
```

The test then reads like intent, not mechanics:

```ts
const loginPage = new LoginPage(page);
await loginPage.goto();
await loginPage.login(process.env.TEST_EMAIL!, process.env.TEST_PASSWORD!);
```

> Whatever you decide — **encode the decision in `skills/test-authoring.md`** so every
> test the agent writes follows the same structure. A pattern is only valuable if it is
> consistent.

---

## Fixtures — log in once, reuse everywhere

Almost every test needs a logged-in user. Repeating the login steps in every test is slow
to run and miserable to maintain. A **fixture** is reusable setup.

```ts
// tests/fixtures.ts
import { test as base } from '@playwright/test';
import { LoginPage } from './pages/LoginPage';

export const test = base.extend<{ authedPage: any }>({
  authedPage: async ({ page }, use) => {
    const loginPage = new LoginPage(page);
    await loginPage.goto();
    await loginPage.login(process.env.TEST_EMAIL!, process.env.TEST_PASSWORD!);
    await use(page);           // hand the logged-in page to the test
  },
});

export { expect } from '@playwright/test';
```

Any test that needs auth just asks for it:

```ts
import { test, expect } from './fixtures';

test('cart total updates when quantity changes', async ({ authedPage }) => {
  // already logged in — go straight to the point
});
```

> **Review signal:** when the agent writes good tests, you see it reach for fixtures
> naturally. When it copy-pastes the same login into twenty tests, that is something to
> fix — **in the skill**, not in the tests.

---

## Isolation — the one beginners miss

**Every test must run on its own, in any order, without depending on another test.**

```ts
// ❌ BAD — test B only passes because test A left an item in the cart
test('A: add item to cart', async ({ page }) => { /* adds item */ });
test('B: cart shows 1 item',  async ({ page }) => { /* assumes A ran */ });

// ✅ GOOD — B sets up its own state
test('B: cart shows 1 item', async ({ authedPage }) => {
  await addItemToCart(authedPage);   // this test's own setup
  await expect(...).toHaveText('1');
});
```

If reordering your tests breaks them, your suite is fragile. Playwright runs tests in
parallel and in arbitrary order — isolation is not optional.

---

## Checklist before you trust the suite

- [ ] No credentials hardcoded anywhere (`grep -r "password123" tests/` returns nothing)
- [ ] `.env` is gitignored; `.env.example` is committed
- [ ] Login happens in a fixture, not copy-pasted
- [ ] Every test sets up its own state
- [ ] Locators are role/label based
- [ ] Every test has a meaningful assertion
- [ ] `npx playwright test` gives the same result three times in a row
