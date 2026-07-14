# Vibetesting in 2026: Automated Testing with Playwright & AI Tools — Course Repository

Resources for the Udemy course. Everything you need to follow along is in this repo.

## Getting started

**Fork this repo** (click **Fork**, top-right) so you have your own copy to commit
to and push from, then clone your fork:

```
git clone git@github.com:<your-username>/playwrightvibetesting.git
cd playwrightvibetesting
```

Full setup — Playwright install, Antigravity, and Playwright MCP — is Section 3.

---

## What's in here

```
playwrightvibetesting/
├── techshop/                 ← The app under test (static HTML/CSS/JS)
│   ├── broken-app/           ← Broken version — 15 bugs planted for testing
│   ├── fixed-app/            ← Fixed version — used in the verification section
│   └── requirements.md       ← Sprint 1 spec — the Section 4 coverage check
│
├── capstone/                 ← Your independent project (Section 12)
│   ├── booknow-broken/       ← Hotel booking app — bugs planted, count not disclosed
│   ├── booknow-fixed/        ← Fixed version — used for your verification phase
│   └── requirements.md       ← BookNow spec — verify the app against it
│
├── tests/                    ← The Playwright suite you build across the course
│   └── (specs land here, flow by flow)
│
├── skills/                   ← The 4 reusable AI Skills built in Section 5
│   ├── test-case-design.md
│   ├── test-authoring.md
│   ├── bug-reporting.md
│   └── flake-triage.md
│
├── docs/                     ← Written guides — the companion to each lecture
│   ├── setup-01-install-tools.md
│   ├── setup-02-fork-and-clone.md
│   ├── setup-03-playwright-mcp.md      ← MCP setup + troubleshooting
│   ├── setup-04-running-the-apps.md
│   ├── s04…s13-*.md                    ← one guide per section
│   └── README.md                       ← index
│
├── prompts/                  ← Every Antigravity prompt, by section + clip
│   ├── section-03-setup.md
│   ├── section-04-exploration.md
│   ├── section-05-skills.md
│   ├── section-06-fundamentals.md
│   ├── section-07-testcases.md
│   ├── section-08-writing-suite.md
│   ├── section-09-bugs.md
│   ├── section-10-stability.md
│   ├── section-11-ci.md
│   └── section-12-capstone.md
│
└── .github/workflows/        ← CI pipeline (Section 11)
    └── playwright.yml
```

> `tests/`, `skills/`, and the workflow start empty — you generate them with the
> AI agent during the course. The finished versions ship as the answer key.

---

## Running TechShop

These are static apps — no server, no build step. Open the file in Chrome, or
serve the folder so Playwright can hit a stable URL:

```bash
cd techshop/broken-app
npx serve .        # or: python3 -m http.server 5500
```

Then point Playwright's `baseURL` at the served address (e.g. http://localhost:3000).

**Test credentials:** `demo@techshop.com` / `password123`

---

## The 15 Bugs (broken-app only) — what the suite targets

| ID | Area | What's wrong | Caught by |
|----|------|-------------|-----------|
| BUG-001 | Login | Password field uses `type="text"` — visible in plaintext | Attribute assertion |
| BUG-002 | Login | Empty email/password accepted | Negative test |
| BUG-003 | Login | Wrong credentials silently redirect to catalog | Negative test |
| BUG-004 | Cart | Discount divides by 1000 not 100 | Computed-total assertion |
| BUG-005 | Cart | Quantity decrements below 1 | Boundary test |
| BUG-006 | Cart | Total does not update on quantity change | State assertion |
| BUG-007 | Catalog | Long descriptions overflow the card | Optional visual assertion |
| BUG-008 | Catalog | "Out of Stock" badge is green, not red | Optional visual assertion |
| BUG-009 | Checkout | Expiry date accepts past dates | Negative test |
| BUG-010 | Checkout | CVV accepts letters/symbols | Input validation test |
| BUG-011 | Checkout | "Proceed to Checkout" unresponsive (blocker) | Navigation assertion |
| BUG-012 | Checkout | Form submits with all fields empty | Negative test |
| BUG-013 | Checkout | Confirmation missing order reference | Presence assertion |
| BUG-014 | General | Browser tab title shows "Untitled" | `toHaveTitle` assertion |
| BUG-015 | General | Navbar visible on login before auth | Visibility assertion |

> **BUG-011 is a blocker** for checkout flows. The "Proceed to Checkout" button
> does not respond to clicks. Note this dependency in any checkout spec — it is a
> realistic example of a bug that blocks other tests.

---

## Credentials — keep secrets out of your code

The suite reads credentials from environment variables. Never hardcode usernames,
passwords, or tokens in spec files, prompts, or commits.

**Local:** copy `.env.example` to `.env`, fill in your values. `.env` is gitignored.

**CI (GitHub Actions):** add `TEST_EMAIL` and `TEST_PASSWORD` as repository secrets.

---

## Playwright MCP Setup (Antigravity)

The agent drives a real browser through the Playwright MCP server.

1. Install Playwright and browsers: `npm init playwright@latest`
2. Add the Playwright MCP server to Antigravity's MCP config
3. Restart Antigravity and confirm the agent can launch a browser

Full step-by-step is in the Section 3 companion article.

---

## Written guides

Every lecture has a written companion in **[`docs/`](docs/README.md)** — the exact
commands, config, templates, and troubleshooting you cannot pause a video to copy.

Start with **[docs/setup-03-playwright-mcp.md](docs/setup-03-playwright-mcp.md)** if MCP
gives you trouble, and **[docs/s11-github-actions-ci.md](docs/s11-github-actions-ci.md)**
when you set up repository secrets.

---

## How to use the prompts

Each file in `prompts/` maps to a course section and lists every prompt with the
exact clip it is used in. Copy the prompt, paste it into the Antigravity chat, and
follow along with the video. Prompts reference the skills in `skills/` and the app
served on http://localhost:3000.

## Course sections and what to find here

| Section | What to grab |
|---------|-------------|
| 3 — Setup | `techshop/broken-app`, `prompts/section-03-setup.md`, MCP config |
| 4 — Exploration | `techshop/broken-app`, `techshop/requirements.md`, `prompts/section-04-exploration.md` |
| 5 — Skills | `prompts/section-05-skills.md` → builds `skills/` |
| 6 — Fundamentals | `prompts/section-06-fundamentals.md` |
| 7 — Test cases | `prompts/section-07-testcases.md` |
| 8 — Writing the suite | `prompts/section-08-writing-suite.md`, `tests/` |
| 9 — Bugs | `prompts/section-09-bugs.md` |
| 10 — Stability | `prompts/section-10-stability.md`, `techshop/fixed-app` |
| 11 — CI | `prompts/section-11-ci.md`, `.github/workflows/playwright.yml` |
| 12 — Capstone | `prompts/section-12-capstone.md`, `capstone/booknow-broken` + `booknow-fixed`, `capstone/requirements.md` |
