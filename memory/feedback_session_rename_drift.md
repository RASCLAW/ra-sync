---
name: Session rename drift detection
description: Proactively suggest /rename when session topic has drifted significantly from its original name. Trigger conditions, examples, anti-nagging rules.
type: feedback
related:
  - feedback_loadout_remote_status.md
  - feedback_multi_session_workflow.md
originSessionId: b42c0c90-aeb4-4688-9e25-4dc8424efb04
---
**Why:** Session 105 started as `niche-strategy-lock` (picking up RAS Creative SOLUTIONS launch prep from backlog #2) and drifted through closeout format audit → multi-window workflow memory → Drive versioning → `/closeout --defer` + `/sendit` architecture → this rule itself. Five unrelated topic shifts. I didn't catch any of them — RA had to notice and name the problem. The session name became stale around the closeout format discussion but I kept treating it as still about niche strategy. This memory exists so future-me catches drift proactively.

## Rule

When the current work clearly differs from the session's named topic, stop and say:

> "Heads up — we started on `<original-topic>` but we're deep in `<current-topic>` now. Want to `/rename`?"

## Trigger conditions (ALL must be true before suggesting)

- **Original topic is parked** — not touched in the last ~5-10 turns
- **Current topic is meaningfully different** — different domain, skill area, output type, or target files. Not just a deep dive on the same thing.
- **Drift was organic, not an announced detour** — if RA said "quick question about X, then back to Y," that's a known tangent, not a rename-worthy shift.
- **No rename suggestion has been made for this drift already** — one suggestion per topic shift, not repeated.

## What IS drift (rename-worthy)

Real examples from session 105:
- Niche strategy lock → log audit diagnostic (adjacent but topic shift)
- Log audit → closeout format feedback memory (different lever entirely)
- Closeout format → Drive versioning script modification (different system)
- Drive versioning → `/closeout --defer` + `/sendit` architecture (different problem class)
- Architecture → session rename drift rule (meta)

Each of those transitions warranted a rename prompt. None got one.

## What is NOT drift (don't suggest)

- Deep dive on the same topic — "we're still on strategy, just at layer 5"
- Quick tangent that returns to main thread — "back to the X question"
- Universal session steps — `/closeout`, `/savepoint`, `/loadout` are not topic changes
- RA explicitly frames something as a side question — "let me ask something real quick"
- Session was never named meaningfully to begin with — that's an initial-naming problem, handled at loadout, not a drift problem

## Anti-nagging rules

- **Suggest once per topic shift.** If RA declines or ignores, drop it. Don't re-suggest for the same drift.
- **Don't suggest on every message.** Wait until the drift is clearly established — 2-3 turns into the new topic, not after the first shift.
- **Don't suggest if RA is visibly trying to finish the current thread.** Let them land the work first.
- **Don't suggest during closeout itself.** Closeout is a universal session step and the existing name is about to be written to PROJECT_LOG anyway.

## How to apply

1. **Track mentally:** what was the session's original named topic at loadout?
2. **Every few turns, self-check:** does current work still match that name? If I had to describe the last 5 turns in 3 words, would that match the session name?
3. **If no AND trigger conditions met:** surface the suggestion using the exact format above.
4. **If accepted:** wait for the new name, use it in closeout, PROJECT_LOG, decisions log.
5. **If declined:** note the decline internally, don't re-suggest for the same drift.

## PROJECT_LOG implications

When a session is renamed mid-stream, the closeout should use the **FINAL** name in PROJECT_LOG. If a session had two clear phases with meaningful work in both (e.g. strategy lock + architecture redesign), consider a dual-phase name like `niche-lock-and-closeout-redesign`. Don't split into two PROJECT_LOG entries — that loses the attribution that it was one conversation.

## Relationship to other memories

- **`feedback_loadout_remote_status.md`:** covers session rename **at start** (initial naming). This memory covers rename **mid-session** (drift correction).
- **`feedback_multi_session_workflow.md`:** mid-session rename becomes especially important in multi-window mode, because attribution across windows relies on meaningful names.
- **`feedback_closeout_format.md`:** one-liner default applies to closeouts regardless of renames — orthogonal rule.
