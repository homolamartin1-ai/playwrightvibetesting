# Section 6 — Playwright Fundamentals & Reading Your First Test

This section is read-only — we do not write new tests here. Instead we read the
**first login spec the test-authoring skill produced in Section 5** and learn to
review it. We use the agent as a tutor and reviewer so that, from here on, you can
read and critique everything it writes.

> Specimen: `playwrightvibetesting/tests/login.spec.ts` (written in Section 5, Prompt 2b).

## Course reference
| Prompt | Used in clip |
|--------|-------------|
| Prompt 1 — Explain your first login test | **Section 6, Clip 1** — Locators, Assertions, and Auto-Waiting |
| Prompt 2 — Tour the project structure and config | **Section 6, Clip 2** — Test Structure, Fixtures, and Config |
| Prompt 3 — Critique the test (the review checklist) | **Section 6, Clip 3** — Reading Tests Critically |
| Prompt 4 — Prove the test can fail | **Section 6, Clip 3** — Reading Tests Critically |

---

## Prompt 1: Explain Your First Login Test
*Used in: Section 6, Clip 1 — "Locators, Assertions, and Auto-Waiting"*

```
Look at the login spec the test-authoring skill wrote in Section 5
(playwrightvibetesting/tests/login.spec.ts). Explain it to me line by line as if I have
never written a test before:
- what each locator finds and why it is or is not a stable locator
- what each assertion actually checks
- where Playwright is auto-waiting, so I understand why there are no sleeps

Keep it plain-language. I want to be able to read any test after this.
```

> If you have not authored `login.spec.ts` yet, do Section 5 Prompt 2b first.

---

## Prompt 2: Tour the Project Structure and Config
*Used in: Section 6, Clip 2 — "Test Structure, Fixtures, and Config"*

Still reading, not writing — a guided tour of how the project is organised.

```
Without changing anything, walk me through the structure of this Playwright
project so I understand how tests are organised:

- the tests/ folder and how spec files are grouped (one file per feature area —
  login.spec.ts now, cart and checkout coming in Section 8)
- the anatomy of the login test: the test block, the page handle, navigate /
  interact / assert
- playwright.config.ts — explain the key settings (baseURL, browsers/projects,
  retries, trace) and which ones we will tune for CI later
- the concept of a fixture for shared setup like login, and where it would live
  (we build the real login fixture in Section 8 — do not create it now)

Read-only: explain, do not generate or modify any files.
```

---

## Prompt 3: Critique the Test (The Review Checklist)
*Used in: Section 6, Clip 3 — "Reading Tests Critically"*

The prompt to internalise — your standard review pass on every test the agent
writes from here on.

```
Review playwrightvibetesting/tests/login.spec.ts critically and report on three things:

1. LOCATORS — are they readable and stable (role/label based), or fragile
   structural chains that will break on a redesign? Flag any fragile ones.

2. ASSERTIONS — does each assertion check something that actually matters,
   or just that the page did not crash? Flag any weak or missing assertions.

3. FALSE GREENS — is there anything that would pass even if the feature were
   broken (waiting on an always-present element, asserting on static text)?

For each issue, suggest the stronger version. Do not change the file yet —
just report. (Because the authoring skill encodes good practice, this test
should mostly pass review — recognising "here is what good looks like" is half
the lesson; the other half is catching it when it does not.)
```

---

## Prompt 4: Prove the Test Can Fail
*Used in: Section 6, Clip 3 — "Reading Tests Critically"*

The habit from the speech: a test that has never failed cannot be trusted.

```
I want to prove a test can actually fail. In playwrightvibetesting/tests/login.spec.ts,
temporarily change one assertion to expect something we know is wrong (e.g. the
wrong page text after login), run the test, and confirm it goes RED. Then revert
the change and confirm it goes GREEN again. Report both results.
```

**Expected:** red when the assertion is wrong, green when correct. If it stays
green both ways, the assertion is meaningless — that is a false green. This is the
move you repeat on every real test the agent writes from here on.
