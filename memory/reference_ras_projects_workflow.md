---
name: reference-ras-projects-workflow
description: "How the ras-projects backlog dashboard's Start prompt works — verdict labels, edit shapes per verdict, smoke-check pattern, settings.json trust setup. Reference companion to [[project-ras-projects-dashboard]]."
metadata: 
  node_type: memory
  related: 
    - project-ras-projects-dashboard
  type: reference
  originSessionId: 82e0b368-e41b-42e2-8d46-7e7b00ce8408
---

# ras-projects · Start prompt workflow

When user pastes a Start prompt from https://ras-projects.pages.dev into a Claude Code session, follow this structure:

## Verdict labels (pick exactly one)

| Label | Meaning |
|---|---|
| STILL_NEEDED | Work hasn't happened, description matches reality. Do the work. |
| ALREADY_DONE | Code/state already reflects what was asked. Confirm + move to Recently Shipped. |
| SCOPE_CHANGED | Partially relevant but description is wrong. Rewrite in place. |
| STALE | No longer meaningful (project pivoted, idea dropped). Delete. |

**Heuristic:** if smoke check passes AND recent commits cover the area → lean ALREADY_DONE. If smoke check fails OR memory still talks about this as open work → lean STILL_NEEDED.

## Investigation depth

1. Smoke check from the section's `> verify:` line
2. `git log --oneline -20` in the project path
3. Project memory dir: `C:\Users\RAS\.claude\projects\c--Users-RAS-projects-<slug>\memory\`
4. Project CLAUDE.md + PROJECT_LOG.md if they exist
5. Don't stop at file-system signals — hit live APIs (Meta Graph, etc.) when they're the source of truth

## Edit shapes per verdict

All edits go to `C:\Users\RAS\projects\ras-projects\BACKLOG.md`.

**ALREADY_DONE** — remove the item line + its indented notes from the project section. Add a single-line DONE entry under `## Recently Shipped`:
```
- [DONE] <task text> — <YYYY-MM-DD>
```

**STALE** — delete the item line + any indented note lines beneath it. **If that empties the section, also remove the `## <project>` header and its `> path:` / `> verify:` lines** (section header auto-cleanup).

**SCOPE_CHANGED** — keep the item line in place. Rewrite the task text and/or indented notes. Adjust priority/effort/status tags as needed (e.g. P1→P2 when scope no longer fits "this week").

**STILL_NEEDED** — do the work in the project. Don't edit BACKLOG.md unless the work is fully done (then becomes ALREADY_DONE flow).

## Rebuild + redeploy (always last)

```
cd C:\Users\RAS\projects\ras-projects
py build.py
npx --yes wrangler@latest pages deploy dist --project-name=ras-projects --branch=main --commit-dirty=true
```

## Report contract

End with a structured report:
1. **Verdict** — one of the 4 labels
2. **What you did** — concrete actions taken
3. **Evidence** — how you confirmed the verdict (file paths, commit hashes, API responses)
4. **For RA** — anything that still needs human input
5. **Related stale entries** — if investigation surfaced stale wording in priority files, project READMEs, MEMORY.md, or other backlog items, flag them here for downstream cleanup

## Settings prerequisite

`C:\Users\RAS\projects\ras-projects` lives in global `~/.claude/settings.json` `permissions.additionalDirectories` so cross-project edits don't trigger permission prompts. Added 2026-05-21.

## Anti-patterns

- Don't commit BACKLOG.md changes to git after each task (no remote, just local commits would clutter). Let `/closeout` batch commits.
- Don't rebuild without redeploying — the dashboard is the user-facing artifact, not the local file.
- Don't move STILL_NEEDED items to DONE unless the work is fully complete + tested.
- Don't preserve the indented note when moving to Recently Shipped — DONE entries are one-liners with dates.
