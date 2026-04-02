---
name: Build Quality Patterns
description: Lessons learned about shipping, iteration, and system reliability
type: feedback
---

Pattern identified (session 66): RA ships v1 and never finishes v2. Missing: monitoring, recovery, validation, iteration.

**Why:** Honest conversation about build quality — multiple systems shipped but not hardened. Morning brief failed silently because env vars don't travel with git repos.

**How to apply:**
- Don't just build and move on. Ask: is this monitored? What happens when it fails?
- No manual DB injection — structured data flows through pipeline only, narrative goes to life-log (Lesson #4, session 67)
- Smoke test before structural changes — verify the system is healthy before making big moves
- Credentials don't travel with git repos — always verify env vars are available in the target environment
- Never break a working system to chase elegance — changes should be incremental and reversible
