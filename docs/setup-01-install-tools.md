# Setup 1 — Install Node, Antigravity & Playwright
**Section 3 · Clips 1 and 3** · Time: ~15 minutes · One-time

Everything here is a one-time setup. If a step fails, do not fight it — paste the
error into the Antigravity chat and ask the agent what went wrong.

---

## 1. Node.js

Playwright runs on Node. Install the **LTS** version.

- Download: **https://nodejs.org** → choose the LTS build for your OS → run the installer.

Verify (any terminal):

```bash
node --version    # expect v20.x or v22.x (any LTS is fine)
npm --version
```

If `node` is "command not found" after installing, **close and reopen your terminal**
(the PATH is only picked up by new shells).

---

## 2. Antigravity

Antigravity is the AI IDE you will live in for the rest of the course.

1. Download and install it from its website.
2. Open it and sign in.

**Important — how you actually work in this course:** you do *not* type commands into
a terminal by hand. You paste prompts into the **agentic chat**, and the agent opens a
terminal in the background and runs the commands for you. Every prompt in
[`prompts/`](../prompts) is written to be pasted into that chat.

---

## 3. Playwright (into the cloned repo)

> Do this **after** [setup-02-fork-and-clone.md](setup-02-fork-and-clone.md) — Playwright
> is installed *inside* the repo you cloned, so the repo becomes your Playwright project.

Use **Prompt 2** in [`prompts/section-03-setup.md`](../prompts/section-03-setup.md), or
run it yourself:

```bash
cd playwrightvibetesting
npm init playwright@latest
```

Answer the prompts:

| Question | Answer |
|---|---|
| TypeScript or JavaScript? | **TypeScript** |
| Where to put tests? | **tests** |
| Add a GitHub Actions workflow? | **No** — we write our own in Section 11 |
| Install Playwright browsers? | **Yes** |

This does three things at once: installs Playwright, downloads the browser engines
(Chromium, Firefox, WebKit), and scaffolds a starter project with a config file and an
example test.

### Verify

```bash
npx playwright test
```

You should see the example test **pass**. That confirms Node, Playwright, and the
browsers are all working.

---

## What just got created

| File / folder | What it is |
|---|---|
| `package.json` | Your project's dependencies |
| `playwright.config.ts` | The control panel — baseURL, browsers, retries, trace |
| `tests/example.spec.ts` | A sample test (we delete or ignore it later) |
| `node_modules/` | Installed packages — **never commit this** (it is gitignored) |

Do not worry about understanding these yet — we read them properly in Section 6.

---

## Troubleshooting

| Symptom | Fix |
|---|---|
| `node: command not found` | Reopen your terminal. Still failing? Reinstall Node LTS. |
| `npm init playwright` hangs on browser download | Slow network — let it finish. It downloads ~3 browser engines. |
| Permission errors on install | Do **not** use `sudo` with npm. Reinstall Node from the official installer. |
| Example test fails | Run `npx playwright install` to (re)download browsers. |

Still stuck? Paste the full error into the Antigravity chat and say: *"This failed while
setting up Playwright — what went wrong and how do I fix it?"*
