---
name: Make.com First Scenario Complete
description: First Make.com scenario (Smart Lead Router with AI Classification) built and tested -- session 79
type: project
---

First Make.com scenario completed and tested on 2026-04-05.

**Scenario:** Smart Lead Router with AI Classification (ID: 5134999)
- Google Forms v2 (watchResponses) -> AI Toolkit v2 (Categorize) -> Router -> Gmail v4 + Google Sheets v2
- Form ID: 1q2uQwnVOer7-Nyk7aBWVAZuENsJ6fITAe4RkEGR4bdw
- Lead Tracker Sheet: 1SMDvErNoGEjTyWKS1XOs3JvGC2b0IoXogpLMpmPNrsQ
- 3-way routing: Hot (email + sheet), Warm (sheet), Cold (sheet)
- AI classification with strict budget-based rules

**Key learnings:**
- MCP can create scenarios but can't update/delete on Free plan
- Always use latest module versions (v2+ for Forms/Sheets, v4 for Gmail)
- Google Forms v2 returns nested answer objects -- must drill to `.textAnswers.answers[].value`
- Make question ID format: `{{1.answers.\`questionId\`.textAnswers.answers[].value}}`
- Custom OAuth needed for Forms scope (Make's default doesn't request it)

**Why:** Phase 1 of portfolio rebuild track. Make.com is first tool in the Make -> Zapier -> n8n -> GHL learning sequence.

**How to apply:** Reference this when building future Make.com scenarios. Module version map and question ID patterns are reusable.
