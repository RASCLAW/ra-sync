---
name: Diagnostic questions — batch, synthesize, pivot
description: When going wide on a diagnostic question, batch tool calls in parallel, synthesize into one report, and pivot on failed paths instead of retrying.
type: feedback
related: [feedback_loadout_remote_status.md, feedback_mid_session_save.md, feedback_sonnet_delegation.md]
originSessionId: d71c4cad-237e-497f-b80e-83b5efe31d27
---
When RA asks a diagnostic question that invites depth ("give me deets", "did it miss something", "full audit"), go wide — but go wide *efficiently*.

**The three rules:**

1. **Batch in parallel, not sequential.** Fire all the independent queries (status endpoint, logs, CRM, config) in one message. Don't run one, read the output, run the next.
2. **Synthesize into one report.** Present a single structured digest at the end: headline numbers → red flags → follow-up options. Do not live-commentate each finding as it arrives.
3. **Pivot on failures, don't retry.** If a tool call fails (network timeout, auth error, wrong endpoint), pivot to an alternative path immediately. Don't retry the same failing approach with minor variations.

**When to stay shallow instead:** If RA asks a narrow status question ("is the chatbot up", "how many messages today"), give the headline only + a menu of deeper paths. Don't preemptively drill.

**Why:** Session 99 — RA asked for chatbot stats with "give me deets". Going deep was legit, but I did it badly: sequential tool calls, live-commentated each finding, retried failed Python google-api-client calls twice (both timed out on local network, same failure mode), then got into billing rabbit holes without synthesizing. RA called it: "youre kinda super slow today." The failure was *how* I went deep, not that I went deep.

**How to apply:**
- First action on a "go wide" prompt: list the independent queries needed, fire them in parallel in one tool-call block
- Only after all results return, write the synthesis
- If a query fails, note it in the report as "couldn't check X" and move on — don't retry unless RA asks
- Exception: narrow status question = surface answer + menu, stay shallow
