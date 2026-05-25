---
name: sendit-external-paths-gap
description: "/sendit hardcodes 3 Drive sync targets -- new content-producing projects (hyperframes renders, future video work) silently miss backup."
metadata: 
  node_type: memory
  type: feedback
  related: 
    - reference_backup_system.md
    - feedback_content_storage_rule.md
  originSessionId: 30e09c3f-c754-43a5-be6a-c9ce486ab1c1
---

When adding a new project that generates content outside `DuberyMNL/contents/`, the content WILL NOT be backed up via `/sendit` by default. The skill hardcodes the 3 sync targets and doesn't walk a config.

**Why:** Discovered 2026-05-23 during a backup audit. `/sendit` had been firing regularly but 168MB of hyperframes renders (`duberymnl-trailer-v1/renders/` + `teka-muna-kinetic-v1/renders/`) were disk-only because they live outside `DuberyMNL/contents/`. Same silent gap will hit any new project: Rasclaw artifacts, future video work, new repo content folders.

**How to apply:**
- **Right now:** after generating content outside `DuberyMNL/contents/`, manually sync it:
  ```
  python tools/drive/sync_folder.py --local <path> --remote "DuberyMNL/backup/<dest>"
  ```
- **Don't trust `/sendit` for non-DuberyMNL content.** It will exit clean and report "complete" while leaving your renders behind.
- **When creating a new content-producing project:** either (a) immediately add a sync line to `~/.claude/commands/sendit.md`, or (b) prioritize the durable fix below.

**Durable fix (backlog):** Convert `/sendit` to read `~/.claude/managed-syncs.json`:
```json
[
  {"local": "c:/Users/RAS/projects/DuberyMNL/contents/new", "remote": "DuberyMNL/backup/contents/new"},
  {"local": "c:/Users/RAS/projects/DuberyMNL/contents/ready", "remote": "DuberyMNL/backup/contents/ready"},
  {"local": "c:/Users/RAS/projects/hyperframes/duberymnl-trailer-v1/renders", "remote": "DuberyMNL/backup/hyperframes-renders/duberymnl-trailer-v1"},
  {"local": "c:/Users/RAS/projects/hyperframes/teka-muna-kinetic-v1/renders", "remote": "DuberyMNL/backup/hyperframes-renders/teka-muna-kinetic-v1"}
]
```
~15-20 min. Adding future paths = JSON edit, no skill rewrite. RA declined doing this 2026-05-23 ("skip for now"); future session should pick it up before next new content project ships.

**Related:** Same shape of gap exists for managed git repos — `/sendit` only pushes 3, leaving 14+ RASCLAW repos to manual pushes. Consider unifying both into a single config file when this gets built.
