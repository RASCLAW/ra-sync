---
name: Mid-session saves always write to memory
description: When RA triggers a mid-session save point, always save something meaningful to memory. Never return just a summary.
type: feedback
related: [feedback_log_doublecheck.md, feedback_diagnostic_depth.md, feedback_closeout_format.md, reference_resume_pointer.md]
originSessionId: d71c4cad-237e-497f-b80e-83b5efe31d27
---
When RA says "mid-session save point", "save point", or similar mid-session checkpoint phrase, **always save at least one meaningful memory entry** on top of the summary.

**Why:** Session 99 — I gave RA a clean mid-session checkpoint summary (what's done, what's in flight, what's next) but offered memory saves as optional ("want me to save..."). RA corrected: "ALWAYS save something to memory. every mid-session save is important. im running mid-session savepoint because I feel like this is worth saving." The act of him calling a save point is itself the signal that something worth preserving happened — he's not asking whether to save, he's asking me to extract the lesson.

**How to apply:**
- On any mid-session save trigger, identify the 1–3 most valuable learnings and write them to memory files
- Candidates to look for: new rules/feedback from the conversation, technical lessons, project decisions with reasoning, reference knowledge that took work to discover
- Do the saves first, then summarize status
- Don't ask "want me to save X?" — just save it. RA can always edit or delete later.
- Exception: if the session genuinely has nothing new (rare), say so explicitly
