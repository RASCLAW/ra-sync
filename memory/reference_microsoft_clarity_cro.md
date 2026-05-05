---
name: Microsoft Clarity + Claude MCP
description: Free funnel CRO stack -- heat maps, session recordings, dead-click tracking, queryable via Claude connector. Pair with UTMs for per-source behavior analysis.
type: reference
related:
  - ../../../../projects/EA-brain/references/summaries/shiver-microsoft-clarity.md
  - ../../../../projects/EA-brain/references/summaries/aaron-young-claude-google-ads.md
  - project_chatbot_recovery_complete.md
  - project_portfolio_rebuild.md
  - reference_competitive_intel.md
originSessionId: 3c07aaf7-382b-42b7-9e77-196737529157
---
**Microsoft Clarity** -- free Microsoft tool, JS snippet on the site. Provides:
- Heat maps (click + scroll attention)
- Full session recordings with UTM attribution
- Dead-click tracking (clicks on non-interactive elements)
- Avg time / scroll depth / sessions by referrer
- Real-time page errors

**Claude Connector** -- in Claude Custom > Connectors, search "Microsoft Clarity" and add. Natural-language queries hit Clarity's data directly.

**Canonical prompts:**
- "Biggest drop-off between headline and CTA on [page], last 7 days"
- "How do paid-ad visitors behave differently than YouTube/organic visitors"
- "Mobile vs desktop breakdown -- conversion rate and drop-off"
- "Differences between converters and non-converters"
- "Where are the dead clicks"
- "What's the biggest bottleneck? What should I do next?"

**Install path:** Google Tag Manager (preferred -- combines with pixel, Hyros, etc. into one snippet for load speed).

**Caveat:** MCP connector is young. Shiver hit a server loading issue mid-demo. Smoke-test before using in client work.

**How to apply:**
- DuberyMNL: install on duberymnl.com before v2 rebuild so we have baseline data. Add to Google Tag Manager alongside existing tags. Expect usable data after ~50 sessions.
- Combine with UTM parameters on Messenger ad links + organic story links so chatbot-driven traffic can be segmented from organic traffic.
- RAS Creative SOLUTIONS: bundle with Aaron Young's ad-side N-gram/SWOT pattern as a full-funnel CRO retainer -- weekly split test + funnel report is a defensible monthly deliverable for solar-installer clients.
- Cadence: one split test per funnel per week, driven by Clarity data.
