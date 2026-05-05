---
name: Closeout format — one-liner decisions + conservative memory linking
description: Default to one-liner decision format in decisions/log.md; upgrade to full ADR only for architectural choices. Don't bidirectionally link memories unless ≥2 topics pre-exist.
type: feedback
related:
  - feedback_log_doublecheck.md
  - feedback_mid_session_save.md
  - feedback_sonnet_delegation.md
originSessionId: b42c0c90-aeb4-4688-9e25-4dc8424efb04
---
**Why:** Session 105 closeout timing diagnosis revealed bidirectional cross-links + default-ADR format are the two real bloat sources in closeout. Session entry length itself is fine (consistent 25-45 lines historically). RA's rule 2026-04-12: "one-liner is fine as long as it doesn't lose too much context."

## Decisions log — default one-liner

Use the three-field format for tactical/reversible decisions:

```
[YYYY-MM-DD] DECISION: ... | REASONING: ... | CONTEXT: ...
```

**Upgrade to full ADR** (Context / Alternatives / Why / Consequences) ONLY when ANY of these is true:
- Architectural or hard-to-reverse (positioning lock, pricing model, framework choice)
- Multiple alternatives were weighed and the "why not" matters for future judgment
- Decision carries "do not drift" level stakes
- One-liner reads as ambiguous or forces future-me to re-derive the reasoning

**Test before shipping a one-liner:** read it back. If a future session would have to ask "what did past-me mean here?", upgrade to ADR.

**Example — should have been one-liner:** Session 105 solar niche decision was written as ~15-line ADR. Tight version:

```
[2026-04-12] DECISION: Solar = RAS CS primary niche + battery paired, strict DuberyMNL gate | REASONING: RA passion + tech learning compounds; highest ticket P200K-P2M; zero PH AI agency competition; fallout leads moat 1:1 fit; battery = same customer, zero forking cost | CONTEXT: session 105 launch prep discussion
```

3 lines vs. 15. Same strategic lock, same "do not drift" anchoring.

**Example — correctly full ADR:** Session 104 positioning lock. Architectural, multiple alternatives weighed (product vs service, one-time vs retainer, brand name), consequences touched every project document. Full ADR was appropriate.

## Memory cross-linking — conservative

**Default: forward-link only** in `related:` field. Don't edit existing memories to back-link unless the topic surface has ≥2 pre-existing related memories that form a genuine hub.

- **0-1 related memories:** forward-link only (write new file with `related:`, skip back-link edits)
- **≥2 related memories:** full bidirectional dance (write new file + edit each target to link back + update MEMORY.md index)

Below the threshold, back-link overhead is wasted work — the link won't compound because the topic cluster isn't dense enough.

## RA's guardrail

**"One-liner is fine as long as it doesn't lose too much context."** The ambiguity test is the line. When in doubt, upgrade to ADR — context loss is worse than format bloat.

## How to apply

- During closeout, draft decisions as one-liner first. Read back. Ship unless ambiguity test fails.
- New memory? Count related memories in frontmatter. 0-1 = forward-link only. ≥2 = full bidirectional.
- Session >2 hours? Use /savepoint mid-session so closeout = consolidation, not composition (per `feedback_mid_session_save.md`).
- This rule doesn't apply to: session entries in PROJECT_LOG (already tight), memory file body content (already topic-bounded), positioning/architectural decisions (need full context).
