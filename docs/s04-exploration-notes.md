# Section 4 — Exploration Notes & Requirements Coverage
**Section 4 · Clips 3–4**

Exploration is only worth doing if you keep what you learned. This guide gives you the
exact structure of `exploration-notes.md` — the file that feeds the test matrix in
Section 7.

---

## The loop

We work **one flow at a time**:

```
explore a flow  →  capture it  →  repeat for the next flow  →  check coverage vs the spec
```

We do **login** together on camera (Prompts 1–2), then you repeat for catalog, cart, and
checkout (Prompt 3), then check the whole thing against the requirements (Prompt 4).

---

## The structure of `exploration-notes.md`

One `##` section per flow. Each contains the same three parts.

```markdown
## Login

### Flows
1. Open http://localhost:3000
2. Enter email and password
3. Click "Sign In"
4. Expect: land on the product catalog

### Selectors
| Element | Locator |
|---|---|
| Email field | getByLabel('Username') |
| Password field | getByLabel('Password') |
| Sign In button | getByRole('button', { name: 'Sign In' }) |
| Error message | getByText('Invalid username or password') |

### Surprises
| # | What I did | Expected | Actual | Breaks requirement |
|---|---|---|---|---|
| 1 | Typed a password | Characters masked | Shown in plaintext | "Password field must mask input" |
| 2 | Submitted empty form | Error shown | Accepted, went to catalog | "Empty fields must be rejected" |
| 3 | Wrong password | Error, stay on login | Redirected to catalog | "Failed login shows error message" |

## Catalog
… same three parts …

## Cart
… same three parts …

## Checkout
… same three parts …
```

---

## Why each part matters

| Part | Why it exists |
|---|---|
| **Flows** | The skeleton your tests will follow — the steps, in order. |
| **Selectors** | Writing tests is largely knowing how to find things reliably. Capturing these now saves hours later. Prefer `getByRole` / `getByLabel` over CSS chains. |
| **Surprises** | Every deviation from expected behaviour. These become your **bug candidates** and your **regression tests** in Section 7. |

---

## The requirements coverage check (Clip 4)

This is what separates *poking around* from *testing*. TechShop ships with a spec:
[`techshop/requirements.md`](../techshop/requirements.md).

Prompt 4 makes the agent do two passes:

### 1. Coverage — did we explore every requirement?
Go rule by rule. Anything not checked yet is a **gap** — go and check it now.

> Example: the spec says *"Orders under $10.00 rejected"*. If you never tried a small
> order, that is a gap, not a pass.

### 2. Compliance — does the app match each rule?
For every requirement you *did* check: **PASS** or **FAIL**, quoting the exact rule.

### Then update the notes
- Add the newly-explored flows
- Tag each **Surprise** with the requirement it breaks (see the table above)
- Add a coverage summary at the bottom:

```markdown
## Coverage Summary
- Requirements checked: 24 / 24
- Passed: 11
- Failed: 12
- Untestable (blocked): 1  ← checkout flow blocked by the dead "Proceed to Checkout" button
```

---

## Why traceability matters

"The password is visible" is an observation.
"The password is visible, which breaks *'Password field must mask input characters'*" is a
**finding** — and a developer and a product owner will both act on it.

It also answers the question every lead asks: *what did you actually cover?*

---

## Common mistakes

| Mistake | Why it hurts |
|---|---|
| Only recording bugs, not flows/selectors | Section 8 gets much slower — you re-discover everything. |
| Letting the agent explore without watching | It raises false alarms and misses real ones. **You confirm each finding.** |
| Capturing all four flows in one giant blob | Keep one `##` section per flow — Section 7 reads it feature by feature. |
| Skipping the coverage check | You end up with tests for what you *happened* to notice, not what the app *promises*. |
