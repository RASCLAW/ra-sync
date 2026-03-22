# ra-sync

Context sync file for ClaudeMob (Claude app on mobile). Updated automatically from Claude Code sessions.

## Files
- `context.md` -- Last 3 days of context (life, work, priorities, pending tasks)

## How It Works
- **Claude Code (PC)** updates `context.md` at every session closeout
- **ClaudeMob (phone)** fetches the raw URL to stay in sync

## Raw URL for ClaudeMob
```
https://raw.githubusercontent.com/RASCLAW/ra-sync/main/context.md
```

## Work Context
For project/code details, ClaudeMob can also fetch from the DuberyMNL repo:
```
https://raw.githubusercontent.com/RASCLAW/DuberyMNL/main/CLAUDE.md
```
