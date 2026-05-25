---
name: project-ras-projects-dashboard
description: Personal backlog dashboard at ras-projects.pages.dev — BACKLOG.md → build.py → Cloudflare Pages. Smart Start button hands Claude an investigate→act→update→redeploy prompt that resolves tasks via three verdict paths (ALREADY_DONE / STALE / SCOPE_CHANGED / STILL_NEEDED). Built 2026-05-21.
metadata: 
  node_type: memory
  related: 
    - reference-ras-projects-workflow
  type: project
  originSessionId: 82e0b368-e41b-42e2-8d46-7e7b00ce8408
---

# ras-projects Dashboard

Personal backlog tracker that pulls reality back into the plan. Lives at **https://ras-projects.pages.dev**, source at `C:\Users\RAS\projects\ras-projects`.

## What it does

- Surfaces ~84 open items across 17 projects with priority (P1/P2/P3) + effort (S/M/L) + status (TODO/WIP/BLOCKED/DONE) tags
- Hero "Today · Focus" panel shows 3 P1 items spread across different projects
- Each item card has 3 actions: **Start** (smart prompt) / **Open** (vscode://) / **Verify** (smoke check)
- Indented context notes under each task line render inline (84/92 items have notes as of v8)

## The Start loop

Click Start → clipboard gets a structured prompt. Paste into any active Claude Code session. Claude:
1. **Investigates** — runs project smoke check, scans git log, searches memory
2. **Picks a verdict**: STILL_NEEDED / ALREADY_DONE / SCOPE_CHANGED / STALE
3. **Acts**: does the work / moves to Recently Shipped / rewrites in place / deletes (with empty-section cleanup)
4. **Updates BACKLOG.md** at the exact line, then `py build.py` + `wrangler pages deploy` to redeploy

Settings: `C:\Users\RAS\projects\ras-projects` is in global `additionalDirectories` so any Claude session can edit + deploy without permission prompts.

## Validation tests (2026-05-21)

3 verdict paths exercised end-to-end:

| Task | Verdict | Outcome |
|---|---|---|
| Unpause boosted ads | ALREADY_DONE | Moved to Recently Shipped; Claude hit Meta Graph API to confirm |
| Marielle Fuse scoring | STALE | Item + indented note + empty section header all removed |
| Tag live ads with ref= | SCOPE_CHANGED | Rewrote in place: P1→P2, TODO→BLOCKED, retitled, full new note |

Each test surfaced *related stale entries elsewhere* (current-priorities.md, MEMORY.md) — the dashboard's gravity pulls cleanup into adjacent surfaces.

## File anatomy

- `BACKLOG.md` — single source of truth. Format: `- [P1][M][TODO] text` followed by optional indented note lines. Sections use `## Project` headers with `> path:` and `> verify:` metadata blockquotes.
- `build.py` — regex parser → JSON embedded in HTML template
- `template.html` + `style.css` — Inter font, dark theme, premium card grid
- `dist/index.html` — generated artifact deployed to CF Pages

## Iterate-deploy loop

```bash
notepad BACKLOG.md
py build.py
npx wrangler pages deploy dist --project-name=ras-projects --branch=main
```

Or click Start on any item and let Claude do it.

## Status

v8 committed 2026-05-21 as `94e468f`. Local-only repo (no GitHub remote). CF Pages project `ras-projects` created via wrangler; production branch `main`.

See [[reference-ras-projects-workflow]] for the Start prompt structure + smoke-check pattern.
