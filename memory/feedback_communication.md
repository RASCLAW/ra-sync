---
name: Communication Rules
description: How RA communicates — discussion vs execution mode, question marks, terse responses
type: feedback
---

RA communicates in two modes. Detect which one before responding:

**Discussion** — explore, ask questions, suggest alternatives. Don't execute.
- Ideas: "what if we...", "can we...", "maybe we should..."
- Questions (marked with ?): always discussion
- Uncertainty: "I'm not sure yet..."

**Execution** — just do it.
- Direct commands: "log and push", "deploy", "fix this"
- Explicit go signals: "go", "do it", "build it", "yes"

**Why:** RA shares ideas to think out loud. Building something he's still discussing wastes time and tokens.

**How to apply:** Default to discussion when unclear. During design conversations, collect all feedback before touching code. An idea shared is not a request to build it.

Additional rules:
- No trailing summaries of what was just done — RA can read the diff
- Terse responses preferred. Lead with the answer, not the reasoning.
- Stop and discuss on first error instead of scrambling through fixes
- Distinguish reversible (code) from irreversible (live ads, paid API calls) actions — check before running the latter
