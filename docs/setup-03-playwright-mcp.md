# Setup 3 — Connect Playwright MCP
**Section 3 · Clip 4** · Time: ~10 minutes · One-time

This is the step that makes the whole course work: it lets the AI agent **drive a real
browser**. It is also the single most likely place to hit friction, so this guide is
deliberately thorough.

---

## What MCP actually is (in one paragraph)

**MCP** (Model Context Protocol) is a standard way to give an AI agent extra tools. The
**Playwright MCP server** is one of those tools: once connected, the agent can open a
browser, click, type, read the page, and report what it sees — live, without writing a
test file first. Your IDE (Antigravity) is the *client*; the Playwright MCP server is
the *tool provider*. You connect them once, and never touch it again.

---

## The easy way: let the agent do it

You do **not** have to edit config by hand. Use **Prompt 3** in
[`prompts/section-03-setup.md`](../prompts/section-03-setup.md) — it tells the agent to:

1. find (or create) Antigravity's MCP configuration file and show you the path,
2. add the `playwright` server entry without touching your existing servers,
3. install the browser (`npx playwright install chromium`),
4. tell you if a **reload/restart** is needed, and
5. verify by opening `example.com` and reporting the page title.

> **The one manual beat:** after the config is written, Antigravity usually needs a
> **reload or restart** to load a newly-added MCP server. The agent cannot hot-load its
> own tools mid-session — it will tell you when to reload, then continue.

---

## The manual way (fallback)

If you would rather wire it yourself, or the agent could not find the config:

### 1. Open Antigravity's MCP settings

Look in Antigravity's **Settings → MCP / MCP Servers** panel. It will either show you the
servers directly or open a JSON config file for editing.

### 2. Add the Playwright server

The entry is the same shape in every MCP client:

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest"]
    }
  }
}
```

**If the file already has other servers**, add `"playwright"` alongside them — do not
replace the whole `mcpServers` block:

```json
{
  "mcpServers": {
    "some-existing-server": { "command": "..." },
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest"]
    }
  }
}
```

> JSON is picky: every entry except the last needs a trailing comma, and no comments are
> allowed. If Antigravity says the config is invalid, that is almost always a stray or
> missing comma.

### 3. Make sure a browser is installed

```bash
npx playwright install chromium
```

### 4. Restart Antigravity

Fully quit and reopen it — not just close the window.

---

## Verify the connection

In the Antigravity chat:

```
Using the Playwright MCP browser tools, open a browser and navigate to
https://example.com. Tell me the page title you see.
```

✅ **Working:** a browser opens, navigates, and the agent reports **"Example Domain"**.

❌ **Not working:** the agent says it has no browser tools, or nothing happens.

---

## Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| Agent says it has no browser/Playwright tools | Server not loaded | Restart Antigravity **fully**. Confirm the config actually saved. |
| Config file "invalid JSON" | Comma/bracket error | Paste the file into the chat and ask the agent to fix the JSON. |
| Browser never opens, no error | Browsers not installed | Run `npx playwright install chromium`. |
| `npx: command not found` | Node not installed / not on PATH | See [setup-01-install-tools.md](setup-01-install-tools.md). |
| Works, then stops after restart | Config written to the wrong file | Ask the agent: *"Show me the exact MCP config file path you edited and print its contents."* |
| Corporate laptop, install blocked | Network/proxy policy | Try a personal machine, or ask IT to allow `npx` package downloads. |

---

## Still stuck? Use the agent

This is the fallback for everything. Paste this into the chat:

```
Playwright MCP is not connecting. Diagnose it for me:
1. Show me the exact MCP config file path you are using and print its contents.
2. Tell me whether the "playwright" server entry is present and valid.
3. Check that Playwright browsers are installed.
4. Tell me exactly what to do next.
```

---

## When it is done

You never touch this again. From Section 4 onward the agent simply *has* a browser, and
the whole course flows from that.
