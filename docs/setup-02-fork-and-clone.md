# Setup 2 — Fork the Repo & Clone Your Fork
**Section 3 · Clip 2** · Time: ~10 minutes · One-time

You need **your own copy** of the course repo — not just a download — because later in
the course you commit your tests and push them to trigger your own CI pipeline.

---

## 1. Fork the repo

1. Open **https://github.com/homolamartin1-ai/playwrightvibetesting**
2. Click **Fork** (top-right) → **Create fork**

You now have `https://github.com/<your-username>/playwrightvibetesting` under your own
account. This is the copy you own and push to.

> **Why fork instead of clone directly?** You cannot push to someone else's repo. In
> Section 11 you push a CI workflow and watch it run — that only works on your own fork.

---

## 2. Clone your fork

Use **Prompt 1** in [`prompts/section-03-setup.md`](../prompts/section-03-setup.md), or
run it yourself. Replace `<your-username>`:

**SSH (recommended):**
```bash
git clone git@github.com:<your-username>/playwrightvibetesting.git
```

**HTTPS (if you have not set up SSH):**
```bash
git clone https://github.com/<your-username>/playwrightvibetesting.git
```

Then confirm you got everything:

```bash
ls playwrightvibetesting
# techshop  capstone  prompts  skills  tests  docs  README.md
```

Open that folder in Antigravity. **From here on it is your workspace** — it is both the
app under test and the home for your tests, skills, and pipeline.

---

## 3. SSH keys (only if you chose SSH)

If `git clone git@github.com:...` fails with *"Permission denied (publickey)"*, you have
no SSH key registered with GitHub. Two options:

### Option A — just use HTTPS
Re-clone with the HTTPS URL above. Perfectly fine for this course. GitHub will ask for
your username and a **personal access token** (not your password) when you push.

### Option B — set up an SSH key (5 minutes, worth it)

```bash
# 1. Create a key (press Enter at every prompt to accept defaults)
ssh-keygen -t ed25519 -C "your-email@example.com"

# 2. Start the agent and add the key
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# 3. Copy the PUBLIC key to your clipboard
#    macOS:
pbcopy < ~/.ssh/id_ed25519.pub
#    Linux:
cat ~/.ssh/id_ed25519.pub
#    Windows (Git Bash):
clip < ~/.ssh/id_ed25519.pub
```

4. On GitHub: **Settings → SSH and GPG keys → New SSH key** → paste → **Add SSH key**

5. Test it:

```bash
ssh -T git@github.com
# Expect: "Hi <your-username>! You've successfully authenticated..."
```

> ⚠️ Only ever paste the **`.pub`** file. Never share the private key (`id_ed25519`).

---

## Troubleshooting

| Symptom | Fix |
|---|---|
| `Permission denied (publickey)` | No SSH key on GitHub — use HTTPS, or do Option B above. |
| `Hi <wrong-user>!` from `ssh -T` | You have multiple GitHub keys and the wrong one is used. Check `~/.ssh/config`. |
| Push rejected: "Repository not found" | You cloned the **original** repo, not your fork. Re-clone from *your* username. |
| HTTPS push asks for a password | GitHub removed password auth — create a **personal access token** and use it as the password. |

---

## Sanity check before moving on

```bash
cd playwrightvibetesting
git remote -v
```

The remote should point at **your** username — not `homolamartin1-ai`. If it does not,
you cloned the wrong repo.
