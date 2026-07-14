# Section 12 — Capstone Brief: BookNow
**Section 12 · All clips** · Your independent project

This is the one where nobody holds your hand. You get a new app, a spec, and your skills.
Everything else is your call.

---

## The app

**BookNow** — a hotel room booking app. Four pages:

```
Login  →  Available Rooms  →  Booking Form  →  Confirmation
```

| Folder | Use |
|---|---|
| `capstone/booknow-broken` | The app under test — **bug count NOT disclosed** |
| `capstone/booknow-fixed` | For your verification pass |
| `capstone/requirements.md` | **The spec — your source of truth for what counts as a bug** |

**Credentials:** `guest@booknow.com` / `stay2026`

**Serve it:**
```bash
cd playwrightvibetesting/capstone/booknow-broken
npx serve . -l 3000      # or: python3 -m http.server 3000
```

---

## ⚠️ Different domain — do not autopilot

BookNow is **not** TechShop with different words. New rules to think about:

- **Dates** — check-out must be *after* check-in
- **Guests** — minimum 1
- **Nights** — calculated from the date range
- **Totals** — discounted nightly rate × number of nights
- **Booking reference** — format `BKN-` + 6 digits

Adapt your prompts. Do not copy TechShop specifics blindly.

---

## Your tasks

Work the same loop you ran all course. The loop **is** the skill.

| # | Task | Adapt from |
|---|---|---|
| 1 | **Explore** with the agent via MCP — all four flows | [`prompts/section-04-exploration.md`](../prompts/section-04-exploration.md) (Prompts 1–3) |
| 2 | **Check coverage vs the spec** — cover gaps, tag findings to requirements | `section-04-exploration.md` (Prompt 4) + `capstone/requirements.md` |
| 3 | **Design test cases** — positive, negative, edge, regression | [`prompts/section-07-testcases.md`](../prompts/section-07-testcases.md) + `skills/test-case-design.md` |
| 4 | **Write the suite** — page objects, fixture, clean locators | [`prompts/section-08-writing-suite.md`](../prompts/section-08-writing-suite.md) + `skills/test-authoring.md` |
| 5 | **Run, triage, report** — real bug vs flaky; file the reports | [`prompts/section-09-bugs.md`](../prompts/section-09-bugs.md) + `skills/flake-triage.md`, `skills/bug-reporting.md` |
| 6 | **Verify** — point the suite at `booknow-fixed`; close out any still-red | [`prompts/section-10-stability.md`](../prompts/section-10-stability.md) |
| 7 | **Ship to CI** — GitHub Actions, headless, artifacts | [`prompts/section-11-ci.md`](../prompts/section-11-ci.md) |

---

## ⭐ The one rule

**Reuse your skills. Do not start from zero.**

The four files in `skills/` are the entire point. You begin the capstone from **leverage**
— exactly like you will begin your next real project.

And if a skill shows a gap on this new domain (it probably will — dates are new):
> **Refine the skill, then continue.** Do not just patch the test.

That habit *is* the course.

---

## Self-check — how to know you did it right

There is no single correct answer. Two good engineers will produce different suites. What
you are looking for is **completeness** and **trustworthiness**.

### Coverage
- [ ] Someone who has never seen BookNow could read your specs and understand what was tested
- [ ] Every flow has positive **and** negative **and** edge cases
- [ ] Every requirement in `capstone/requirements.md` is either covered or explicitly noted as not covered

### The tests themselves
- [ ] Clean role/label locators — no fragile CSS chains
- [ ] Meaningful assertions — not "the page did not crash"
- [ ] **Every failing test fails for the right reason** → break the app on purpose and confirm it goes red

### Bug reports
- [ ] Each one is reproducible by someone who was not there
- [ ] Expected and actual are separate fields
- [ ] Severity is stated **and justified**
- [ ] Each ties back to a requirement

### Stability
- [ ] The suite gives the **same result on three consecutive runs**

### CI
- [ ] Green against `booknow-fixed`
- [ ] Meaningful red against `booknow-broken`
- [ ] Report and traces downloadable as artifacts either way

---

## When you are done

You will have a **second** complete project — a different domain, done independently, with
its own suite, reports, and pipeline. That is the one you walk an interviewer through,
because you can say honestly: *nobody showed me how to do this one.*

> A walkthrough of a completed BookNow capstone is in the course resources.
> **Watch it after your own attempt, not before.** Watching first is copying the answer.
