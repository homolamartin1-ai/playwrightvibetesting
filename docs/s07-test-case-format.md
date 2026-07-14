# Section 7 — Test Case & Test Matrix Format
**Section 7 · All clips**

This is the format the `test-case-design` skill produces, and what `test-cases.md` should
look like before you write a single line of Playwright.

---

## The format of one test case

| Field | Purpose |
|---|---|
| **ID** | Stable reference — `LOGIN-N01`. Bug reports and specs point back to it. |
| **Title** | Describes *behaviour*, not a click sequence. |
| **Category** | Positive · Negative · Edge · Regression |
| **Preconditions** | State the test needs before it starts. |
| **Steps** | Numbered, unambiguous. |
| **Expected result** | What *should* happen — per the requirements. |
| **Requirement** | The rule it verifies (from `techshop/requirements.md`). |

### Example

```markdown
### LOGIN-N02 — Login is rejected when the password is empty
- **Category:** Negative
- **Preconditions:** On the login page, not authenticated
- **Steps:**
  1. Enter `demo@techshop.com` in the username field
  2. Leave the password field empty
  3. Click "Sign In"
- **Expected:** An error message is shown and the user stays on the login page
- **Requirement:** "Empty fields must be rejected with an error message"
```

---

## The four categories — and why each exists

| Category | Proves | Where the bugs actually are |
|---|---|---|
| **Positive** | The feature works when used correctly | The easy half. Necessary, but rarely where bugs hide. |
| **Negative** | Invalid input is **rejected** | 🔥 Where most real bugs live. Handling bad input correctly is genuinely hard. |
| **Edge** | Boundaries hold | 🔥 Where assumptions break — zero, below-minimum, past dates, max length. |
| **Regression** | A **known bug** stays dead | Written from confirmed bugs. Fails on broken, passes on fixed. |

> If a feature has only positive cases, your coverage is not done. You are testing that
> the app works when everyone behaves — which is not the world your users live in.

---

## Titles: behaviour, not clicks

| ❌ Bad | ✅ Good |
|---|---|
| "Test login" | "Login succeeds with valid credentials" |
| "Click the button" | "Cart total updates when quantity changes" |
| "Check checkout" | "Checkout rejects a card expiry date in the past" |

A reader should know what is being verified without reading the steps.

---

## Regression cases (Clip 3)

A **regression test** is written from a confirmed bug and asserts the **correct**
behaviour — so it *fails today and passes once fixed*, and can never silently come back.

```markdown
### CART-R01 — Discount is calculated as a percentage (regression)
- **Category:** Regression  ·  **Related bug:** discount divides by 1000, not 100
- **Preconditions:** Logged in, one item in the cart priced $100 with a 10% discount
- **Steps:**
  1. Open the cart
- **Expected:** The line total shows **$90.00**
- **Requirement:** Cart shows correct line total and order total
- **Note:** FAILS against broken-app (shows $9.99…). PASSES against fixed-app.
```

**The professional habit:** every confirmed bug becomes a permanent test.
Bugs fixed without a regression test have a habit of coming back. Bugs with a test
guarding them stay dead.

---

## Blocked tests

Some cases cannot run because another bug blocks them — in TechShop, the dead
**"Proceed to Checkout"** button blocks the whole checkout flow.

**Do not delete or skip them silently.** Write them, and mark the dependency:

```markdown
- **Blocked by:** "Proceed to Checkout" button does not respond (checkout unreachable via UI)
- These tests stay RED until that blocker is fixed.
```

Documenting reality is the job. Pretending the flow works is not.

---

## What `test-cases.md` should look like when you are done

```markdown
# TechShop — Test Cases

## Login
### LOGIN-P01 — Login succeeds with valid credentials  (Positive)
### LOGIN-N01 — Login is rejected when both fields are empty  (Negative)
### LOGIN-N02 — Login is rejected when the password is empty  (Negative)
### LOGIN-N03 — Login shows an error for wrong credentials  (Negative)
### LOGIN-R01 — Password field masks its input  (Regression)

## Catalog
…

## Cart
…

## Checkout
…
```

---

## Your job as the editor

The skill generates a strong first draft. **You** make it complete:

- Did any feature get only positive cases?
- Is there a TechShop-specific negative case a generic template would miss?
  (card expiry in the past, CVV with letters, quantity below 1, order under $10)
- Does every planned regression trace back to a confirmed bug?

> **Coverage is a decision, not an accident.** Senior testers decide what *not* to test,
> on purpose, and can explain why.
