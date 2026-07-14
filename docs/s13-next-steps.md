# Section 13 — Your Toolkit & Next Steps
**Section 13 · All clips**

You finished. Here is how to package what you built, and where to go next.

---

## What you actually walk away with

| Artifact | Where it lives |
|---|---|
| A complete Playwright suite | `tests/` in your fork |
| Your **AI Skills pack** | `skills/` — four files you carry to every future project |
| Structured bug reports | `bug-reports/` |
| A working CI pipeline | `.github/workflows/playwright.yml` |
| A capstone project | Your BookNow suite, done independently |

**Make the repo public.** It is your portfolio. A hiring manager can read it in five
minutes.

---

## Putting it on your CV

Be **specific**. Evidence beats buzzwords.

| ✅ Write this | ❌ Not this |
|---|---|
| "Built a Playwright E2E suite (TypeScript) covering login, cart, and checkout flows; caught 15 defects." | "Familiar with Playwright" |
| "Set up a GitHub Actions pipeline running the suite headless on every push, archiving HTML reports and traces." | "Knows CI/CD" |
| "Use an agentic IDE with Playwright MCP to explore apps and generate E2E tests, with reusable skill files that keep output consistent." | "Uses AI tools" |

That last sentence describes a way of working most candidates **cannot describe yet**.
It is your differentiator — do not water it down to "AI-assisted testing."

### Skills worth naming
`Playwright` · `TypeScript` · `E2E / UI automation` · `GitHub Actions` · `CI/CD` ·
`Page Object Model` · `Playwright MCP` · `AI-assisted test generation` · `Test design
(positive/negative/edge)` · `Defect reporting`

---

## Where to go deeper in Playwright

| Topic | Why it matters | Start here |
|---|---|---|
| **Network mocking** (`page.route`) | Test without a live backend; force error states you cannot otherwise reproduce | playwright.dev → Network |
| **Auth state reuse** (`storageState`) | Log in once, reuse the session across the whole suite — big speed win | playwright.dev → Authentication |
| **Parallelism & sharding** | Keep a large suite fast | playwright.dev → Parallelism |
| **Visual comparison** (`toHaveScreenshot`) | Catch UI regressions functional assertions miss | playwright.dev → Visual comparisons |
| **Component testing** | Test UI pieces in isolation | playwright.dev → Components |
| **API testing** (`request` fixture) | Seed data and assert on the backend from the same suite | playwright.dev → API testing |

Official docs: **https://playwright.dev** — genuinely excellent, and the best next read.

---

## Keep sharpening your skills (literally)

Your four skill files are **living documents**. Every project you run them on, refine
them. A mature, battle-tested skill set — sharpened across dozens of projects — is
portable expertise you take from job to job.

> Six months from now, the skills you refine on real work will be dramatically better
> than the drafts you wrote in Section 5.

---

## Practise

- Take a public web app, or one of your own side projects, and build a suite for it
- Put it on GitHub with a working CI badge
- Do it again with a different domain — that is how the instinct compounds

---

## Communities

| Community | What it is |
|---|---|
| **Ministry of Testing** | The largest community of professional testers |
| **Playwright Discord** | Active, welcoming, maintainer-attended |
| **Playwright GitHub Discussions** | Where the hard questions get answered |

Your questions after this course have been answered there many times.

---

## The rest of the Vibetesting series

| Course | Status |
|---|---|
| **Manual** Vibe Testing | Live — where the story starts (same TechShop app) |
| **API** Vibe Testing | Live — the backend half (Postman, Bruno, pytest, Robot Framework) |
| **Playwright** Vibe Testing | This one — the browser half |
| Mobile · Visual · Performance · AI Vibe Testing | Coming |

Between manual, API, and UI, you now have the full picture — all with the same AI-first
workflow.

---

## The last thing

The tools will keep changing. Antigravity will improve. Playwright will add features.
Prompts that work today will work differently in a year.

**What will not change** is the underlying skill: understanding what you are testing,
knowing what correct behaviour looks like, and being able to verify whether the software
delivers it.

That is the human part. That is the part that scales with you.

> You are not a user of AI tools. You are a tester who **directs** AI tools with judgment.
> **The AI is fast. You are right.**
