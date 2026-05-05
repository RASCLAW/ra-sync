---
name: Verify Data Before Reporting
description: Always query the source data file before writing any number in a report -- never write from memory or estimation
type: feedback
related: [project_team_faith_dashboard.md, feedback_diagnostic_depth.md]
originSessionId: bdfc8809-ed9c-49f3-ba17-93036b6b3ba8
---
Never write numbers in a report, brief, or summary without first reading them from the actual source (JSON, sheet, API response).

**Why:** In the Team Faith supervisor brief, fabricated team total (9,060 vs actual 10,164), wrong agent unit count (Jhayson 518 vs 512), and vague accuracy ("below 99.95%" vs 99.9158%) were all caught after the PDF was already sent. RA's words: "we can't be guessing data, this is real live data and i dont want mapahiya."

**How to apply:** Before writing any stat, score, count, or percentage into a document intended for a third party:
1. Run a quick Python/JS snippet against the actual data file to pull the exact number
2. Use that output verbatim — no rounding from memory, no estimates
3. This applies even when the number "feels right" — feelings about data are not data
