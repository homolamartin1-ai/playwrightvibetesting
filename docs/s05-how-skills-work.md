# Section 5 — How Skills Work
**Section 5 · All clips** · The centerpiece of the course

Skills are the one genuinely new idea in this course, so this guide explains what they
actually *are*, where they live, and how the agent uses them.

---

## What a Skill is

A **Skill** is a plain markdown file containing your standards, written down once, that
the agent reads *before* doing a task.

That is it. There is no magic format and no framework.

### The problem it solves

Without a skill, you re-prompt from scratch every time:

> *"Write me a login test"* → you get **something**. But its quality depends entirely on
> how you phrased it today. Tomorrow you phrase it slightly differently and get different
> locator conventions, a different file structure, a different idea of what a good
> assertion is.

With a skill, you write the standard once:

> *"Following `skills/test-authoring.md`, write the login spec"* → you get the **same
> conventions every time**, regardless of how you phrased the request, how tired you are,
> or which day it is.

### The analogy
Verbal instructions to a new teammate every morning **vs.** handing them a written
playbook. The playbook scales. The verbal instructions do not.

---

## Where skills live

```
playwrightvibetesting/
└── skills/
    ├── test-case-design.md   ← feature → test matrix
    ├── test-authoring.md     ← test cases → Playwright code
    ├── bug-reporting.md      ← failure + trace → developer-ready report
    └── flake-triage.md       ← failure → real bug or flaky test?
```

They are committed to your repo. They are **yours** — you carry them to the next project
and the next job.

---

## How the agent actually uses one

There is no special loading mechanism. You simply **reference the file in your prompt**:

```
Following playwrightvibetesting/skills/test-authoring.md and the LOGIN cases in
test-cases.md, write playwrightvibetesting/tests/login.spec.ts.
```

The agent reads the skill file, then follows its rules while it writes. That is the whole
mechanism.

> **Tip:** if the agent ignores a rule, the rule is usually vague. Skills work best when
> they are **specific and testable** — "prefer `getByRole` and `getByLabel`; never use CSS
> descendant chains" beats "use good locators".

---

## Anatomy of a good skill

A skill should say **what to do**, **what not to do**, and **what the output must look
like**. Here is the shape:

```markdown
# Skill: Test Authoring

## Purpose
Turn test cases into Playwright tests to a consistent standard.

## Rules
- LOCATORS: prefer getByRole / getByLabel. Never use brittle CSS or structural chains.
- ASSERTIONS: every test must assert the real outcome (state, value, visibility) —
  never just "the page did not crash".
- STRUCTURE: one spec file per feature area. Use a light Page Object per area.
- FIXTURES: shared setup (login) goes in a fixture, never copy-pasted.
- ISOLATION: each test sets up its own state and runs in any order.
- SECRETS: credentials come from env vars (TEST_EMAIL / TEST_PASSWORD). Never hardcode.

## Forbidden
- No manual sleeps / waitForTimeout — rely on auto-waiting.
- No hardcoded credentials.
- No assertions on values that never change.

## Output
A .spec.ts file that satisfies every rule above, plus a short note on what it covers.
```

Short, specific, enforceable. That is a good skill.

---

## The four skills you build

| Skill | Turns… | …into | Used from |
|---|---|---|---|
| `test-case-design.md` | a feature + exploration notes | a positive/negative/edge test matrix | Section 7 |
| `test-authoring.md` | test cases | Playwright specs | Sections 6, 8 |
| `bug-reporting.md` | a failure + trace | a developer-ready bug report | Section 9 |
| `flake-triage.md` | a failure | "real bug" or "flaky test" + a fix | Sections 9, 10 |

---

## ⭐ The most important idea in the whole course

**Skills are living documents, not write-once config.**

When the agent produces something wrong, you have a choice:

| Option | What happens |
|---|---|
| ❌ Patch the one test | The same mistake comes back next week, and the week after. |
| ✅ **Fix the skill**, then regenerate | Fixed **everywhere, permanently**, and on every future project. |

We deliberately hit this wall in **Section 8, Clip 4** and refine a skill on camera.

So every time the agent gets something wrong, ask:
> *Is this a one-off, or is this a gap in my skill?*

Almost always, it is the skill.

---

## Common mistakes

| Mistake | Fix |
|---|---|
| Skill is a vague wish-list ("write good tests") | Make each rule specific and checkable. |
| Skill written once and never touched again | Refine it every time it falls short. That is the point. |
| Forgetting to reference the skill in the prompt | The agent will not read it unless you name the file. |
| One giant skill for everything | Keep them separate — each does one job well. |
