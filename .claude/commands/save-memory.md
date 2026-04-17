Scan this conversation. Do TWO things:

## 1. Session Summary

Create or append to `memory/sessions.md` with a short high-level summary of this session. Format:

```
### YYYY-MM-DD — [2-5 word title]
- [What we did, 3-8 bullet points, brief but specific]
- [Include key outcomes, not just tasks]
- [Note anything left unfinished or to follow up on]
```

This file is for quickly resuming context in a new chat. Keep entries concise — no more than 10 lines per session.

## 2. Persistent Memory

Extract and save anything worth keeping across sessions. Follow the two-tier memory architecture:

**MEMORY.md is the index — keep it under 150 lines. Never dump detail there.**

### Routing rules:

| Memory type | Where it goes |
|-------------|---------------|
| One-liner fact, preference, or pointer | `memory/MEMORY.md` — add to the right section |
| Detailed MCP config, gotchas, connection patterns | `memory/mcp-servers.md` |
| VPS, OpenClaw agents, Jerry infra | `memory/openclaw.md` |
| Hooks, rules, CLI tools, commands | `memory/infrastructure.md` |
| Epsillon Media Hub app details | `memory/epsillon-hub.md` |
| Reusable dev patterns or workflows | `memory/patterns.md` |
| Client-specific context | `clients/[name]/memory/` — decisions.md, learnings.md, or context.md |
| New topic with 10+ lines of detail | Create a new `memory/[topic].md` file and add a one-line pointer in MEMORY.md |

**Before writing:** Check if an existing entry covers the topic — update it rather than append a duplicate.

**After writing:** Check MEMORY.md line count. If it's approaching 150 lines, migrate the largest section to a topic file.

Add today's date as a heading when appending. Never overwrite existing entries.

## 3. Agent Memory (if agents were used this session)

If any agents with persistent memory were invoked during this session, prompt the user to update those agents' memories with what was learned.

### Agents with `memory: project` (stored in `.claude/agent-memory/`)

These accumulate learnings specific to Epsillon Media's business. Update when the session involved:

| Agent | Update when... |
|-------|---------------|
| `marketing-brain` | New marketing strategy decisions, orchestration patterns that worked |
| `mkt-copy` | Brand voice corrections, copy formats that landed well, formats to avoid |
| `mkt-ads` | Campaign performance data, audience learnings, what worked/failed |
| `mkt-seo` | Keyword wins, content that ranked, on-page patterns |
| `mkt-analytics` | Tracking setup decisions, attribution gotchas |
| `mkt-strategy` | Strategic pivots, competitor insights, positioning decisions |
| `mkt-research` | Content sources that consistently deliver, angles that resonate |
| `electrical-wizz-brain` | EW-specific campaign data, audience insights, compliance notes |

To update: invoke the agent and say _"Update your memory with what we learned this session: [summary]"_

### Agents with `memory: user` (stored in `~/.claude/agent-memory/`)

These accumulate cross-project learnings. Update when the session surfaced reusable patterns:

| Agent group | Update when... |
|-------------|---------------|
| `dev-brain`, `dev-frontend`, `dev-backend`, `dev-automation`, `dev-qa`, `dev-devops`, `dev-design` | Architecture decisions, stack-specific gotchas, patterns that saved time |
| `code-reviewer`, `code-simplifier`, `code-architect`, `code-explorer` | Codebase conventions, false-positive patterns to suppress, anti-patterns seen |
| `silent-failure-hunter`, `pr-test-analyzer`, `comment-analyzer`, `type-design-analyzer` | Recurring issues found, project-specific suppression rules |
| `build-error-resolver`, `tdd-guide`, `database-reviewer`, `refactor-cleaner`, `e2e-runner` | Error patterns solved, test strategies used, DB schema decisions |
| `audit-meta`, `audit-google`, `audit-budget`, `audit-creative`, `audit-tracking`, `audit-compliance` | Platform-level audit findings, client-agnostic patterns, benchmark data |
| `planner`, `dev-campaign` | Planning patterns, estimation gotchas, campaign analysis frameworks |

To update: invoke the agent and say _"Update your memory with what you learned from this session."_

### When to skip agent memory updates

- Session was purely conversational (no agent work done)
- Agent only did a single routine task with no novel learnings
- The learning is already covered by Section 2 above (use those routes instead)