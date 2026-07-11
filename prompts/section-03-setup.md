# Section 3 — Setup

All prompts and commands used in the setup section. Paste prompts into the
Antigravity chat; the agent runs the commands and reports back.

## Course reference
| Prompt | Used in clip |
|--------|-------------|
| Prompt 1 — Fork and clone the course repo | **Section 3, Clip 1** — Fork and Clone the Course Repo |
| Prompt 2 — Install Playwright and browsers | **Section 3, Clip 2** — Installing Node, Playwright, and the Browsers |
| Prompt 3 — Verify the Playwright MCP connection | **Section 3, Clip 3** — Installing Antigravity + Connecting Playwright MCP |
| Prompt 4 — Serve TechShop locally | **Section 3, Clip 4** — Running TechShop Locally |
| Prompt 5 — Your first vibe test | **Section 3, Clip 5** — Your First Vibe Test |

> MCP config and account setup are step-by-step in `udemy-article-mcp-setup.md`.
> Fork from: **https://github.com/homolamartin1-ai/playwrightvibetesting**

---

## Prompt 1: Fork and Clone the Course Repo
*Used in: Section 3, Clip 1 — "Fork and Clone the Course Repo"*

First, on GitHub, open **https://github.com/homolamartin1-ai/playwrightvibetesting**
and click **Fork** (top-right) to create your own copy under your account. This is
the copy you will commit to and push from later in the course.

Then clone **your** fork. Paste this into the Antigravity terminal, replacing
`<your-username>` with your GitHub username:

```
# SSH (recommended):
git clone git@github.com:<your-username>/playwrightvibetesting.git

# or HTTPS:
# git clone https://github.com/<your-username>/playwrightvibetesting.git

ls playwrightvibetesting
```

**Expected:** the `playwrightvibetesting` folder now exists locally and `ls` shows
`techshop`, `capstone`, `prompts`, `skills`, `tests`, and `README.md`. Open this
folder (or the folder that contains it) as your Antigravity workspace — it is both
the app under test and where your tests, skills, and pipeline will live.

---

## Prompt 2: Install Playwright and Browsers
*Used in: Section 3, Clip 2 — "Installing Node, Playwright, and the Browsers"*

Paste this into the Antigravity chat:

```
Set up Playwright inside the playwrightvibetesting repo I just cloned. Run each
step in the terminal and wait for my confirmation before continuing.

1. Confirm Node is installed:
   node --version
   (If this fails, stop and tell me to install Node LTS from nodejs.org.)

2. Move into the repo and initialise Playwright with the defaults:
   cd playwrightvibetesting
   npm init playwright@latest
   - TypeScript: yes
   - tests folder: tests
   - GitHub Actions workflow: no (we add our own in Section 11)
   - install browsers: yes

3. Confirm the install worked by running the example test:
   npx playwright test

I should see the example test pass. Report the result back to me.
```

---

## Prompt 3: Verify the Playwright MCP Connection
*Used in: Section 3, Clip 3 — "Installing Antigravity + Connecting Playwright MCP"*

Use this **after** you have added the Playwright MCP server to Antigravity's MCP
config and restarted the app (see `udemy-article-mcp-setup.md`).

```
Using the Playwright MCP browser tools, open a browser and navigate to
https://example.com. Tell me the page title you see.

If you cannot open a browser, the Playwright MCP server is not connected —
tell me so I can check my MCP configuration.
```

**Expected:** a browser window opens, navigates to example.com, and the agent
reports the title "Example Domain". If nothing opens, the MCP connection is not
live — recheck the config and restart Antigravity.

---

## Prompt 4: Serve TechShop Locally
*Used in: Section 3, Clip 4 — "Running TechShop Locally"*

```
Serve the broken TechShop app so Playwright has a stable URL to target.
Run this in the terminal and keep it running:

   cd playwrightvibetesting/techshop/broken-app && npx serve . -l 3000

Then confirm it is reachable:
   curl -s -o /dev/null -w "%{http_code}" http://localhost:3000

A 200 means it is up. Tell me the result. Leave the serve process running
in this terminal for the rest of the course.
```

**Test credentials (from env, never hardcoded):** `demo@techshop.com` / `password123`

---

## Prompt 5: Your First Vibe Test
*Used in: Section 3, Clip 5 — "Your First Vibe Test"*

TechShop must be running on http://localhost:3000 and Playwright MCP connected.

```
Using the Playwright MCP browser, do the following on http://localhost:3000:

1. Open the page and tell me what you see.
2. Find the login form, enter the email demo@techshop.com and the password
   password123, and submit it.
3. Tell me exactly what happened after you submitted — which page you landed
   on, and anything that looked wrong or unexpected.

Do not write any test files yet. Just drive the browser and report back.
```

**Expected:** the agent opens TechShop, logs in, and narrates the result. This
confirms the whole chain — Node, Playwright, browsers, Antigravity, MCP, and the
app — is working. If it stalls, the cause is almost always the MCP connection or
the app URL.
