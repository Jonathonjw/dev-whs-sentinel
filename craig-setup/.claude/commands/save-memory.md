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
| Hooks, rules, CLI tools, commands | `memory/infrastructure.md` |
| Reusable dev patterns or workflows | `memory/patterns.md` |
| New topic with 10+ lines of detail | Create a new `memory/[topic].md` file and add a one-line pointer in MEMORY.md |

**Before writing:** Check if an existing entry covers the topic — update it rather than append a duplicate.

**After writing:** Check MEMORY.md line count. If it's approaching 150 lines, migrate the largest section to a topic file.

Add today's date as a heading when appending. Never overwrite existing entries.
