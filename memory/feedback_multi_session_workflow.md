---
name: Multi-window session workflow
description: Best practices for running multiple Claude Code windows in parallel without file collisions, git conflicts, or orphan pileup. Consolidates lessons from sessions 99, 102, 105.
type: feedback
related:
  - feedback_multi_session_safety.md
  - reference_vscode_extension_orphan_bug.md
  - feedback_loadout_remote_status.md
  - feedback_session_rename_drift.md
originSessionId: b42c0c90-aeb4-4688-9e25-4dc8424efb04
---
**Why:** Session 105 had 3 parallel sessions on 2026-04-12 (105/106/107) all touching PROJECT_LOG.md + current-priorities.md + MEMORY.md. It worked but got lucky on push ordering. This memory consolidates the rules so future-me doesn't have to re-derive them under stress.

## Pre-flight (before starting parallel sessions)

- **Pick orthogonal workstreams per window.** The #1 rule. Good split: Window A = chatbot infra, Window B = content generation, Window C = strategy/memory. Bad split: 3 windows all editing `cloud-run/knowledge_base.py`. If two windows share a topic, you'll collide.
- **Run loadout in every window.** Each session gets its own tunnel check + orphan audit. The `pc-status.ps1 -SessionsOnly` output shows current session count.
- **Name sessions early** via `/rename` (e.g. "chatbot-recovery", "content-pipeline", "niche-strategy"). Makes attribution cheap when something goes wrong — `pc-status` shows meaningful labels instead of PIDs.

## During work

- **File ownership rule** (detailed in `feedback_multi_session_safety.md`): if `git status` shows a file you didn't touch, another session owns it. Don't stage, stash, or edit.
- **Stage specific files by name** in every commit. Never `git add -A` or `git add .`. Sweep commands vacuum up other sessions' mid-edits.
- **Heavy shared-file work = single session only.** No parallel sessions during schema migrations, knowledge_base rewrites, or bulk refactors.
- **Read-before-edit goes stale fast.** If a session has been open >1 hour and another window may have touched the same file, re-read before Edit — the `old_string` may no longer match.
- **Commit small, commit often.** Frequent commits flush state so other sessions see current reality when they pull.

## Closeout coordination

**Preferred pattern (session 105+): deferred push via `/closeout --defer` + `/sendit`**

For multi-window sessions, use the deferred push pattern instead of sequential closeouts:

1. **In each window, run `/closeout --defer`** when finishing that window's work. This commits locally but skips push, secret backup, AND Drive content sync. Fast, race-free.
2. **At AFK time, run `/sendit`** in any window. This pushes all 3 managed repos (DuberyMNL, ~/.claude, EA-brain), backs up secrets, syncs `contents/new` and `contents/ready`. One command, ships everything.

**Benefits:**
- Zero push races across parallel sessions — only `/sendit` pushes, and only once
- Faster per-session closeouts (skip all cloud operations until ready)
- Cleaner commit history — each session's commits land together at flush time

**Cost:**
- Small safety net reduction: if PC crashes between `/closeout --defer` and `/sendit`, unpushed commits and un-synced content are lost (commits stay in local git, so recoverable via manual push; Drive sync content is lost until next session regenerates or manually pushes).
- One extra command to remember at AFK time.

**Fallback pattern (if `/sendit` not an option):**

- **Close sessions sequentially when possible.** 3 parallel closeouts = race conditions on PROJECT_LOG.md, MEMORY.md, current-priorities.md, decisions/log.md.
- **If parallel closeout is unavoidable:**
  1. Expect push rejections. Run `git pull --rebase origin <branch>` before pushing.
  2. Verify work post-close: re-read PROJECT_LOG.md, MEMORY.md, current-priorities.md. Confirm your session's entries / memories / priority updates actually landed.
  3. If another session overwrote yours, recreate from conversation context — the JSONL transcript persists on disk.
- **Use absolute file paths in Edit and git staging.** Relative path errors compound under multi-session stress. Session 105 had a wrong-path failure (MEMORY.md at wrong root) that would have been worse with active races.

## Mid-session rename on topic drift

If your session's topic drifts significantly from its original name (backlog item → unrelated architecture, for example), you should be prompted to `/rename` mid-session. See `feedback_session_rename_drift.md` for the full rule. This matters extra in multi-window mode because cross-window attribution relies on meaningful names — a stale name defeats the point of naming in the first place.

## Git state recovery

- **Rejected push →** `git pull --rebase origin <branch>` → resolve conflicts if any → `git push`. Never force.
- **Never force-push in multi-session mode.** You'd destroy another session's commits permanently. If you think you need force-push, stop and ask RA.
- **Unknown uncommitted files = leave alone.** Don't `git checkout`, `git stash`, or `git restore` on anything you didn't write. Another session owns it.
- **Branch divergence:** if your branch is 2 commits behind origin, another session pushed while you were working. Pull-rebase to fold your work on top; push again.

## Orphan cleanup

- **Use `/clear` before closing a VSCode tab**, not the X button. X leaks a claude.exe process holding 300-600MB RAM + state. See `reference_vscode_extension_orphan_bug.md`.
- **Kill orphans promptly** when loadout flags them via `Stop-Process -Id <pid>,<pid> -Force`. Don't let them accumulate — session 99 lost work from orphan pileup.
- **JSONL transcripts persist on disk.** If you killed a session prematurely, `claude --resume` can restore it. No real data loss from killing an orphan.

## When NOT to multi-session

Force yourself to single-window for:
- Memory system restructure (lint, archive, cross-ref audit)
- Git history rewrites (rebase, cherry-pick, filter-branch)
- Infrastructure migrations (DNS, tunnel, deploy pipeline, OAuth scopes)
- Large refactors touching many shared files
- Session when fatigued — window confusion compounds

## How to apply

1. **At session start:** pick a topic that doesn't overlap with other active windows. `/rename` the session.
2. **Before any commit:** run `git status` and mentally filter out files owned by other sessions. Stage only your own work by explicit name.
3. **Before editing a file that's been sitting idle:** re-read it. Multi-session drift is silent.
4. **On closeout:** if another session is also closing out, go second and expect pull-rebase. Verify your work landed after push.
5. **On orphan detection:** kill via `Stop-Process`, don't leave them.
6. **Single-window tasks:** do memory audits, refactors, infra migrations only when you can guarantee no other windows are writing.

## Tonight's scorecard (sessions 105/106/107 on 2026-04-12)

**Worked:** distinct topics, no force-pushes, all 3 session entries survived in PROJECT_LOG.md, multi-session safety rule caught me before touching cloud-run/.
**Lucky:** push ordering worked out, MEMORY.md updates didn't collide on same lines, no duplicate memory filenames.
**Could improve:** announce workstream-per-window explicitly at start, close sequentially not simultaneously, keep a mental map of file ownership.
