# Sentinal WHS — Craig's Claude Code Setup

## Prerequisites

1. **Claude Code** installed — download from [claude.ai/code](https://claude.ai/code)
2. **Node.js 20+** installed
3. **Git** installed and configured with your GitHub account
4. **Access to the repo** — ask Jonathan to add you as a collaborator on GitHub

---

## Step 1: Clone the repo

Clone the Sentinal WHS repo to your machine:

```bash
git clone https://github.com/Jonathonjw/WHS-Sentinal.git "Sentinal WHS"
cd "Sentinal WHS"
npm install
```

Note the full path where you cloned it (e.g. `C:\Users\Craig\repos\Sentinal WHS`).

---

## Step 2: Create your Claude Code workspace

1. Create a folder for your workspace (separate from the repo itself):
   - e.g. `C:\Users\Craig\Claude Code Workspaces\Sentinal-WHS\`

2. Copy the contents of this `craig-setup/` folder into your workspace:
   - `CLAUDE.md` → `[workspace]/CLAUDE.md`
   - `.claude/` → `[workspace]/.claude/`

3. **Update paths in `CLAUDE.md`:**
   - Find `[YOUR_REPO_PATH]` → replace with the full path to where you cloned the repo
   - Find `[YOUR_WORKSPACE_PATH]` → replace with the path to your workspace folder

---

## Step 3: Open in Claude Code

1. Open Claude Code (desktop app or VS Code extension)
2. Open your workspace folder (`[workspace]/`)
3. Claude Code will pick up the `CLAUDE.md` and `.claude/` config automatically

---

## Step 3b: Install the memory system

Claude Code stores project memory in a user-level config folder keyed to your workspace path. After opening Claude Code at least once in your workspace:

1. Find your project memory folder. It will be at:
   ```
   C:\Users\[YourUsername]\.claude\projects\c--Users-[YourUsername]-Claude-Code-Workspaces-Sentinal-WHS\memory\
   ```
   The folder name is your workspace path with backslashes → `--` and spaces removed.

2. Copy the contents of `.claude/memory/` from this setup package into that folder:
   ```
   [workspace]/.claude/memory/*.md  →  C:\Users\[YourUsername]\.claude\projects\...\memory\
   ```

3. Verify by opening a new Claude Code session in your workspace — Claude should have full Sentinal context without you explaining anything.

---

## Step 4: Configure Supabase MCP (optional but recommended)

The Supabase MCP lets Claude run database migrations directly. To set it up:

1. Get the Supabase access token from Jonathan (or create one at [supabase.com/dashboard/account/tokens](https://supabase.com/dashboard/account/tokens))
2. Create `.mcp.json` in your workspace folder:

```json
{
  "mcpServers": {
    "supabase": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@anthropic-ai/supabase-mcp@latest"],
      "env": {
        "SUPABASE_ACCESS_TOKEN": "your-token-here"
      }
    }
  }
}
```

> Keep `.mcp.json` out of git — it's already in `.gitignore`.

---

## Step 5: Set up your git identity

```bash
git config user.name "Craig"
git config user.email "craig@youremail.com"
```

---

## Collaboration workflow

Jonathan and Craig both work on the same repo. To avoid conflicts:

**Craig always uses feature branches:**

```bash
# Start new work
git checkout master
git pull origin master
git checkout -b feature/craig-[description]

# When done
git push origin feature/craig-[description]
# Then open a PR on GitHub for Jonathan to review
```

**Jonathan reviews and merges to master.** Railway auto-deploys from master.

**Never push directly to master** — if you're there by accident, use `/push-work` and it'll ask you to create a branch first.

---

## Available commands

These slash commands are available in your Claude Code session:

| Command | What it does |
|---------|-------------|
| `/pull-work` | Pull latest from master safely |
| `/push-work` | Commit + push (branch-aware, guards against accidental master push) |
| `/plan` | Plan a feature before touching code |
| `/commit` | Create a clean git commit |
| `/commit-push-pr` | Commit, push, and open a PR in one step |
| `/build-fix` | Fix TypeScript/build errors |
| `/code-review` | Review your changes |
| `/verify` | Run verification checks |
| `/autonomous` | Toggle autonomous mode (Claude acts without asking permission) |

---

## Project overview

See `CLAUDE.md` for full project context: module list, architecture decisions, build status, and patterns to follow.

**Key things to know:**
- Stack: Next.js + TypeScript + Tailwind CSS v4 + Supabase
- Deploy: push to master → Railway auto-deploys (usually 2-3 min)
- Supabase project: `wuoasccjyqkdstsxuudt`
- DB migrations live in `supabase/migrations/` — apply via Supabase MCP or Supabase CLI

---

## Quick start session

Open Claude Code in your workspace and try:

```
/pull-work
```

Then describe what you want to build or fix. Claude knows the entire Sentinal architecture from `CLAUDE.md`.
