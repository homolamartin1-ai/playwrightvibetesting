# Setup 4 — Running TechShop & BookNow
**Section 3 · Clips 5–6** (and reused in Sections 10 and 12) · Time: ~5 minutes

Both apps are **static** — plain HTML, CSS, and JavaScript. There is no backend and no
build step. But Playwright needs a stable URL to point at, so we *serve* the folder.

---

## The four app folders

| Folder | What it is | Used in |
|---|---|---|
| `techshop/broken-app` | TechShop with **15 planted bugs** | Sections 4–9 (most of the course) |
| `techshop/fixed-app` | TechShop, bugs resolved | Section 10 (regression pass) |
| `capstone/booknow-broken` | BookNow, bugs planted (**count not disclosed**) | Section 12 |
| `capstone/booknow-fixed` | BookNow, bugs resolved | Section 12 (your verification) |

**Only ever serve one at a time, on port 3000.** The whole course assumes
`http://localhost:3000`.

---

## Serve an app

```bash
cd playwrightvibetesting/techshop/broken-app
npx serve . -l 3000
```

Leave that terminal running. Open **http://localhost:3000** — you should see the TechShop
storefront.

### If `npx serve` grabs a random port

`serve` sometimes ignores `-l 3000` and prints something like
`Accepting connections at http://localhost:62767`. That means **port 3000 was already
taken** and it silently fell back. Use Python instead — it binds reliably or fails loudly:

```bash
cd playwrightvibetesting/techshop/broken-app
python3 -m http.server 3000
```

---

## ⚠️ The #1 gotcha: a stale server on port 3000

**Symptom:** you changed something, or you are testing the broken app, but the page shows
the *old* content or the *wrong* app.

**Cause:** another server is already holding port 3000 — often one you forgot you started,
or one from a different folder/clone.

### Find out what is on port 3000

**macOS / Linux:**
```bash
lsof -iTCP:3000 -sTCP:LISTEN -n -P
```

To see *which folder* it is serving (this is the useful bit):
```bash
for p in $(lsof -tiTCP:3000 -sTCP:LISTEN); do
  ps -o command= -p $p
  lsof -a -p $p -d cwd -Fn | grep '^n' | sed 's/^n/  serving: /'
done
```

**Windows (PowerShell):**
```powershell
Get-NetTCPConnection -LocalPort 3000 -State Listen |
  Select-Object OwningProcess |
  ForEach-Object { Get-Process -Id $_.OwningProcess }
```

### Kill it

```bash
# macOS / Linux
kill -9 $(lsof -tiTCP:3000 -sTCP:LISTEN)
```
```powershell
# Windows
Stop-Process -Id <PID> -Force
```

Then confirm it is free and start your server again.

> **Sanity check that saves hours:** if the page looks wrong, do not debug your test —
> first confirm *which app* port 3000 is actually serving.

---

## Credentials

| App | Username | Password |
|---|---|---|
| TechShop | `demo@techshop.com` (or `admin`) | `password123` |
| BookNow | `guest@booknow.com` | `stay2026` |

⚠️ **Never hardcode these in test files.** From Section 8 on they come from environment
variables — see [s08-suite-structure-and-env.md](s08-suite-structure-and-env.md).

---

## Point Playwright at the app

In `playwright.config.ts`:

```ts
use: {
  baseURL: 'http://localhost:3000',
}
```

Then tests can navigate with relative paths — `await page.goto('/')`.

---

## Troubleshooting

| Symptom | Fix |
|---|---|
| `localhost:3000` shows "cannot connect" | No server running. Start one (above). |
| Page shows the **wrong app** or old content | A stale server owns :3000. Find and kill it (above). |
| `serve` prints a random port | Port 3000 taken — kill the squatter, or use `python3 -m http.server 3000`. |
| Tests fail with `net::ERR_CONNECTION_REFUSED` | The app is not being served. Start it, then re-run. |
| App loads but looks unstyled | You served the wrong folder — `cd` into the app folder itself, not its parent. |
