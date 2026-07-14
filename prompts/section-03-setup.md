# Section 3 — Setup

> 📖 **Guides for this section:**
> [Install the tools](../docs/setup-01-install-tools.md) ·
> [Fork & clone](../docs/setup-02-fork-and-clone.md) ·
> [**Playwright MCP**](../docs/setup-03-playwright-mcp.md) ·
> [Running the apps](../docs/setup-04-running-the-apps.md)

All prompts and commands used in the setup section. Paste prompts into the
Antigravity chat; the agent runs the commands and reports back.

## Course reference
| Prompt | Used in clip |
|--------|-------------|
| Prompt 1 — Fork and clone the course repo | **Section 3, Clip 2** — Fork and Clone the Course Repo |
| Prompt 2 — Install Playwright and browsers | **Section 3, Clip 3** — Install Playwright into the Repo |
| Prompt 3 — Verify the Playwright MCP connection | **Section 3, Clip 4** — Connect Playwright MCP |
| Prompt 4 — Serve TechShop locally | **Section 3, Clip 5** — Running TechShop Locally |
| Prompt 5 — Your first vibe test | **Section 3, Clip 6** — Your First Vibe Test |

> **Setup order:** Clip 1 installs Node + Antigravity, so from Prompt 1 on you paste everything into Antigravity's terminal/chat.

> MCP config and account setup are step-by-step in `docs/setup-03-playwright-mcp.md`.
> Fork from: **https://github.com/homolamartin1-ai/playwrightvibetesting**

---

## Prompt 1: Fork and Clone the Course Repo
*Used in: Section 3, Clip 2 — "Fork and Clone the Course Repo"*

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
*Used in: Section 3, Clip 3 — "Install Playwright into the Repo"*

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

## Prompt 3: Set Up and Verify Playwright MCP
*Used in: Section 3, Clip 4 — "Connect Playwright MCP"*

The agent configures the MCP server itself — you do not edit config by hand.

```
Set up the Playwright MCP server in Antigravity for me — do not make me edit any
config by hand — then verify it works. Work step by step and tell me what you do.

1. Find Antigravity's MCP configuration file for my operating system (create it
   if it does not exist). Show me the path.
2. Add an MCP server named "playwright" that runs the official Playwright MCP
   server, without removing any servers already in the file:
       command: npx
       args:    ["@playwright/mcp@latest"]
3. Make sure the browser is installed: run  npx playwright install chromium
4. Tell me clearly whether Antigravity needs a reload or restart to load the new
   server. If it does, tell me to reload now, then wait for me to confirm.
5. Once the Playwright MCP browser tools are available, verify the connection:
   open a browser to https://example.com and report the page title back to me.

If any step fails, stop and tell me exactly what failed and what to try next.
```

**Expected:** the agent writes the MCP config, prompts you to reload if needed,
then opens example.com and reports the title **"Example Domain"** — that confirms
the connection is live. If it cannot open a browser after the reload, the server
did not load — have it re-check the config path and the entry it added.

> Fallback: if you would rather wire the config manually, the exact entry and
> file paths are in `docs/setup-03-playwright-mcp.md`.

---

## Prompt 4: Serve TechShop Locally
*Used in: Section 3, Clip 5 — "Running TechShop Locally"*

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
*Used in: Section 3, Clip 6 — "Your First Vibe Test"*

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
