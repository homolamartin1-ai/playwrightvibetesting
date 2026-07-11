# Section 8 — Writing the Test Suite

Build the suite flow by flow with the test-authoring skill — and refine the skill
on camera when it hits its limits.

## Course reference
| Prompt | Used in clip |
|--------|-------------|
| Prompt 1 — Set up the login fixture | **Section 8, Clip 2** — Auth, Fixtures, and Test Data Isolation |
| Prompt 2 — Write the login spec | **Section 8, Clip 3** — Building Tests Flow by Flow |
| Prompt 3 — Write the cart spec | **Section 8, Clip 3** — Building Tests Flow by Flow |
| Prompt 4 — Write the checkout spec (with blocker) | **Section 8, Clip 3** — Building Tests Flow by Flow |
| Prompt 5 — Refine the skill when it falls short | **Section 8, Clip 4** — Hitting a Wall: Refining the Skill |

> All prompts follow `playwrightvibetesting/skills/test-authoring.md` and use
> `playwrightvibetesting/test-cases.md`. TechShop (broken) runs on http://localhost:3000.

---

## Prompt 1: Set Up the Login Fixture
*Used in: Section 8, Clip 2 — "Auth, Fixtures, and Test Data Isolation"*

```
Following playwrightvibetesting/skills/test-authoring.md, create a Playwright fixture that
logs in once and provides an authenticated page to any test that needs it.

- Put it in playwrightvibetesting/tests/fixtures.ts
- Read credentials from env vars TEST_EMAIL and TEST_PASSWORD (no hardcoding)
- Expose an authenticated `page` fixture tests can request
- Also create playwrightvibetesting/.env.example listing TEST_EMAIL and TEST_PASSWORD

Explain how a test will use the fixture so it does not repeat the login steps.
```

---

## Prompt 2: Write the Login Spec
*Used in: Section 8, Clip 3 — "Building Tests Flow by Flow"*

```
Following playwrightvibetesting/skills/test-authoring.md and the LOGIN cases in
playwrightvibetesting/test-cases.md, write playwrightvibetesting/tests/login.spec.ts.

Use a light LoginPage page object for locators/actions. Cover the positive,
negative, edge, and regression login cases. Then run it:
   npx playwright test login.spec.ts
Report which tests pass and which fail, and confirm the failures match the known
login bugs (plaintext password, empty fields accepted, wrong creds redirect).
```

**Expected:** negative/regression tests go RED against the broken app — that red
is the suite working. Review locators and assertions before moving on.

---

## Prompt 3: Write the Cart Spec
*Used in: Section 8, Clip 3 — "Building Tests Flow by Flow"*

```
Following the authoring skill and the CART cases in test-cases.md, write
playwrightvibetesting/tests/cart.spec.ts with a CartPage object. Cover: adding items,
changing quantity, the below-1 boundary, the discount calculation, and the order
total updating. Use the login fixture so each test starts authenticated and
isolated. Run it and report results.
```

**Expected:** the discount-math and negative-quantity tests fail against broken —
as designed.

---

## Prompt 4: Write the Checkout Spec (Handle the Blocker)
*Used in: Section 8, Clip 3 — "Building Tests Flow by Flow"*

```
Following the authoring skill and the CHECKOUT cases in test-cases.md, write
playwrightvibetesting/tests/checkout.spec.ts with a CheckoutPage object. Cover card
expiry validation, CVV validation, empty-form submission, and the confirmation
order reference.

IMPORTANT: the "Proceed to Checkout" button is a known blocker — it does not
respond to clicks, which blocks reaching the checkout page through the UI. Write
the tests for the full flow anyway, but add a clear comment marking the blocker
dependency and note that these tests will pass once the blocker is fixed. Do not
fake or skip the flow silently. Run it and report results.
```

---

## Prompt 5: Refine the Skill When It Falls Short
*Used in: Section 8, Clip 4 — "Hitting a Wall: Refining the Skill"*

The key teaching moment — fix the skill, not just the test.

```
Looking across the login, cart, and checkout specs you just wrote, identify any
place where your output was weak or inconsistent — for example a fragile locator
on the checkout form, or repetitive page-object code.

Do NOT just patch the individual test. Instead, update
playwrightvibetesting/skills/test-authoring.md with a new rule that prevents this class of
problem in future. Show me the diff to the skill. Then regenerate the affected
spec using the improved skill and confirm it is better.
```

**Expected:** an improved `test-authoring.md` plus a regenerated spec. This is the
"skills are living documents" lesson made real — and it pays off in the capstone.
