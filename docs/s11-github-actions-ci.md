# Section 11 — GitHub Actions CI
**Section 11 · All clips**

This is the jump from *"I write tests"* to *"I run a quality gate."* After this, your
suite runs on every push — headless, in the cloud, whether you are at your desk or asleep.

> This only works on **your own fork** (see [setup-02-fork-and-clone.md](setup-02-fork-and-clone.md)).
> You cannot push a workflow to someone else's repo.

---

## 🔑 Step 1 — Add your repository secrets (do this FIRST)

Your tests read credentials from environment variables. Locally that comes from `.env`.
**In CI there is no `.env` file** — the values come from GitHub secrets.

1. Go to **your fork** on GitHub
2. **Settings** (repo settings, not your account settings)
3. Left sidebar → **Secrets and variables** → **Actions**
4. Click **New repository secret**
5. Add these two, one at a time:

| Name | Value |
|---|---|
| `TEST_EMAIL` | `demo@techshop.com` |
| `TEST_PASSWORD` | `password123` |

> ⚠️ **Names must match exactly** — they are case-sensitive and referenced literally in
> the workflow YAML.

> 🔒 Secrets are write-only. Once saved you cannot read them back, and they are
> **automatically masked** in logs. If a secret ever appears in a log, GitHub replaces it
> with `***`.

---

## Step 2 — The workflow file

It lives at `.github/workflows/playwright.yml`. Use **Prompt 1** in
[`prompts/section-11-ci.md`](../prompts/section-11-ci.md) to have the agent write it —
then **review it** against this reference:

```yaml
name: Playwright Tests

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    timeout-minutes: 30

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: lts/*

      - name: Install dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps

      - name: Serve TechShop
        run: npx serve techshop/fixed-app -l 3000 &

      - name: Wait for the app
        run: npx wait-on http://localhost:3000

      - name: Run Playwright tests
        run: npx playwright test
        env:
          TEST_EMAIL: ${{ secrets.TEST_EMAIL }}
          TEST_PASSWORD: ${{ secrets.TEST_PASSWORD }}

      - name: Upload report and traces
        uses: actions/upload-artifact@v4
        if: always()          # ← upload even when tests FAIL (that is when you need them)
        with:
          name: playwright-report
          path: |
            playwright-report/
            test-results/
          retention-days: 30
```

### What each part does

| Step | Why |
|---|---|
| `checkout` | Gets your code onto the clean cloud machine |
| `setup-node` | Installs Node (nothing is pre-installed) |
| `npm ci` | Installs dependencies exactly as locked |
| `playwright install --with-deps` | Downloads browsers **plus** the Linux system libraries they need |
| Serve the app | The tests need something at `localhost:3000` |
| `wait-on` | Do not start testing before the server is actually up |
| `env:` block | The **only** place credentials enter — from secrets |
| `if: always()` | **Critical.** Without it, artifacts are skipped when tests fail. |

---

## Step 3 — Commit and push

```bash
git add .github/workflows/playwright.yml
git commit -m "Add Playwright CI workflow"
git push
```

The moment that file lands, GitHub notices it and your pipeline is live.

---

## Step 4 — Watch the run

1. Your repo → **Actions** tab
2. Click the run at the top
3. Watch the steps execute live
4. On the commit itself you get a **green check ✅** or **red X ❌**

### Download the evidence

On the run summary page, scroll to **Artifacts** → download `playwright-report`. Inside:

- `playwright-report/index.html` — open it in a browser for the full HTML report
- `test-results/**/trace.zip` — open with `npx playwright show-trace <file>`

This matters because the CI machine is **thrown away** the moment the run finishes. The
artifacts are the only evidence left.

---

## Headless — why you do not see a browser

There is no screen on a CI server, so browsers run **headless** (no visible window). Same
browser, same engine, same behaviour — just invisible. Playwright does this automatically
in CI. **Your tests do not change.**

---

## Troubleshooting CI

| Symptom | Cause | Fix |
|---|---|---|
| `ERR_CONNECTION_REFUSED` | Tests started before the app was up | Add the `wait-on` step |
| Browsers fail to launch on Linux | Missing system libraries | Use `npx playwright install --with-deps` |
| Auth tests fail only in CI | Secrets missing/misnamed | Check **Settings → Secrets** — names are case-sensitive |
| No artifacts on a failed run | Missing `if: always()` | Add it to the upload step |
| `npm ci` fails | No `package-lock.json` committed | Commit the lockfile |
| Passes locally, fails in CI | An assumption your machine quietly satisfied | See below 👇 |

### "Passes locally, fails in CI" — the most common surprise

This is almost always an **environment difference**: a file that exists only on your
machine, a timing difference on a slower runner, or a dependency you installed globally
and forgot.

Download the trace and hand it to the agent (**Prompt 4**):

```
This test passed locally but failed in GitHub Actions. Here is the CI log and the
downloaded trace. Tell me whether this is (a) a real bug, (b) a flaky test, or (c) an
environment difference between my machine and the clean CI runner — and propose the fix.
```

---

## What you just crossed

Tests that run **on every change, without you remembering to run them.** A lot of people
can write a test. Far fewer can wire it into a pipeline that fails the build when it
should and archives the evidence when it does.

Put that on your CV — specifically:

> *"Built a GitHub Actions pipeline running a Playwright suite headless on every push,
> with HTML reports and traces archived as artifacts."*
