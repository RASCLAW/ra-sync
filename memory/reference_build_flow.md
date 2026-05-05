---
name: Build Flow System
description: Custom Superpowers-inspired build flow -- orchestrator rule + 4 skills (brainstorm, plan, execute, debug) + verification gate. Cherry-picked from Nate Herk's video, avoids plugin context bloat.
type: reference
related: [reference_youtube_skill.md, feedback_claude_code_layers.md]
originSessionId: 60513586-10b6-4daf-84d2-72b1a3c48439
---
Built in session 116 (2026-04-13). Cherry-picked from Superpowers plugin (github.com/obra/superpowers) instead of installing wholesale (14 extra skills = context bloat).

**Components:**
- `~/.claude/rules/build-flow.md` -- orchestrator rule, auto-fires on non-trivial builds
- `~/.claude/skills/brainstorm/` -- visual companion, localhost dashboard with clickable options + server.py
- `~/.claude/skills/plan/` -- hyper-detailed plans to .tmp/plan.md (2-5 min tasks, file paths, acceptance criteria)
- `~/.claude/skills/execute/` -- task-by-task with safety stops, subagent dispatch, post-task review
- `~/.claude/skills/debug/` -- 4-phase: investigate > analyze > hypothesize > fix
- Verification gate wired into `/closeout` (step 4b) and `/pipeline` (step 7)

**Flow:** request > clarify > /brainstorm > /plan > /execute > verify > done

**Why custom instead of plugin:** RA has 34 domain skills. Adding 14 generic Superpowers skills would increase context cost. The orchestrator as a rule file (~40 lines) costs far less than 14 skill description loads. Skills can be invoked individually or as the full chain.

**Source:** [Nate Herk summary](~/projects/EA-brain/references/summaries/nate-herk-superpowers-plugin.md)
