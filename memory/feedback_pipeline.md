---
name: Pipeline & Tool Lessons
description: Hard-won lessons from building the DuberyMNL content pipeline across 71 sessions
type: feedback
---

**kie.ai quirks:**
- Drive URLs don't work as image_input — must pre-upload to kie.ai CDN first (session 37)
- lh3.googleusercontent.com/d/{ID} format works for direct CDN access (session 36)
- Logo URL broke API (redirected to Google login) — use local files instead (session 37)
- Resolution default: 2K (was 1K, changed session 46)

**Why:** These caused days of debugging. kie.ai silently fails on bad URLs.

**Subprocess gotchas:**
- `claude --print` subprocess blocks if CLAUDECODE env var is set — must unset in subprocess env (session 35)
- `claude --print` subprocess pauses for tool permission with no interactive terminal — needs --dangerously-skip-permissions (session 36)

**Why:** Both caused silent failures where the process exited 0 without doing work.

**Pipeline data rules:**
- Google Sheet is source of truth for manual edits; pipeline.json is local cache (session 47)
- Always sync before and after operations — stale data causes duplicate rows
- fcntl file locking on pipeline.json to prevent race conditions (session 41)
- No manual DB injection — structured data through pipeline only (session 67, Lesson #4)

**Prompt quality:**
- Over-prescribed overlay rules fight NB2's creative output — keep rules surgical (session 7)
- Prompt writer original (skill_original.md) produced better results than over-engineered version (session 7)
- Product fidelity rule: never describe frame color/texture from product name — reference image is only authority (session 38)
- Gatekeeper subprocess is too lenient vs main context — known limitation (session 39)

**How to apply:** When touching the pipeline, check these lessons before making changes. Don't repeat the same debugging cycles.
