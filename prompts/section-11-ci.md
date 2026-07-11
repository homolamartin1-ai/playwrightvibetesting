# Section 11 — Shipping to CI with GitHub Actions

The agent drafts the workflow; you review it (especially secrets) and ship it.

## Course reference
| Prompt | Used in clip |
|--------|-------------|
| Prompt 1 — Generate the GitHub Actions workflow | **Section 11, Clip 2** — The Agent Writes the Workflow |
| Prompt 2 — Review the workflow (secrets + steps) | **Section 11, Clip 2** — The Agent Writes the Workflow |
| Prompt 3 — Commit and trigger the first run | **Section 11, Clip 3** — Running Headless in CI |
| Prompt 4 — Debug a CI failure from artifacts | **Section 11, Clip 4** — AI-Assisted Debugging of CI Failures |

---

## Prompt 1: Generate the GitHub Actions Workflow
*Used in: Section 11, Clip 2 — "The Agent Writes the Workflow"*

```
Create a GitHub Actions workflow at course-repo/.github/workflows/playwright.yml
that, on every push and pull request:

1. checks out the repo
2. sets up Node LTS
3. installs dependencies (npm ci) and Playwright browsers
   (npx playwright install --with-deps)
4. serves the TechShop fixed app on port 3000 in the background
5. runs the Playwright suite (it runs headless automatically in CI)
6. uploads the HTML report AND traces as artifacts, even if the run fails

Read credentials from GitHub repository secrets TEST_EMAIL and TEST_PASSWORD via
the env: block — never hardcode them. Explain each section after you write it.
```

---

## Prompt 2: Review the Workflow (Secrets + Steps)
*Used in: Section 11, Clip 2 — "The Agent Writes the Workflow"*

Never commit CI config you have not read. Make the agent justify it.

```
Walk me through course-repo/.github/workflows/playwright.yml and confirm:
- the trigger (push + PR) is correct
- the steps are in a sensible order (install before run, app served before tests)
- browsers are installed with --with-deps
- credentials come ONLY from secrets.TEST_EMAIL / secrets.TEST_PASSWORD, never
  hardcoded or echoed to logs
- the report and traces are uploaded with if: always() so we get them on failure

Flag anything that does not meet these and fix it.
```

> Repo secrets: GitHub → repo → Settings → Secrets and variables → Actions → add
> `TEST_EMAIL` and `TEST_PASSWORD`.

---

## Prompt 3: Commit and Trigger the First Run
*Used in: Section 11, Clip 3 — "Running Headless in CI"*

```
Commit the workflow and push it so the pipeline runs:
   git add course-repo/.github/workflows/playwright.yml
   git commit -m "Add Playwright CI workflow"
   git push

Then tell me what to watch for in the GitHub Actions tab — how to see the run,
read the green/red status on the commit, and download the report and trace
artifacts from the run summary.
```

---

## Prompt 4: Debug a CI Failure From Artifacts
*Used in: Section 11, Clip 4 — "AI-Assisted Debugging of CI Failures"*

For the classic "passes locally, fails in CI" case.

```
A test passed on my machine but failed in the GitHub Actions run. Here is the
downloaded trace and the CI log: [attach/point to them].

Read the trace and the log and tell me whether this is:
- a real bug the change introduced,
- a flaky test (use flake-triage.md), or
- an environment difference between my machine and the clean CI runner
  (missing dependency, timing, the app not being ready when tests started).

Then propose the specific fix.
```

**Expected:** a diagnosis grounded in the trace, plus a concrete fix. You are
never stuck staring at a red build — you pull the evidence and reason through it.
