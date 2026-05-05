---
name: Session Resume Pointer
description: Latest savepoint snapshot -- read first on any new session to pick up where we left off
type: project
originSessionId: b63c8355-4735-47ae-8aa8-c08f92fa6397
---

# Resume Pointer

**Session:** 6 -- reports-and-pdf-pipeline (2026-05-05 23:30 UTC+8)
**Status:** IN PROGRESS

## Current topic
Team Faith Dashboard — supervisor brief PDF sent; waiting on Marielle's Fuse scoring confirmation from supervisor

## Last action
Added note to SUPERVISOR_BRIEF.md that Marielle's 54.0% includes Fuse (43.2% CMS-only); PDF regenerated but NOT resent — user paused to confirm with supervisor

## Next action
Once RA confirms whether Fuse counts toward CMS productivity goal: if YES → 54% stands; if NO → update brief to 43.2% and regenerate PDF via `node .tmp/make_sup_pdf.cjs && python .tmp/send_sup_tg.py`

## Key files
- `C:/Users/RAS/projects/team-dashboard/SUPERVISOR_BRIEF.md` — supervisor report; Marielle's entry shows 54.0% (43.2% CMS-only); pending Fuse policy confirmation
- `C:/Users/RAS/projects/team-dashboard/DASHBOARD_REPORT.md` — builder-facing analysis report (11 sections)
- `C:/Users/RAS/projects/team-dashboard/.tmp/make_sup_pdf.cjs` — MD→HTML→PDF pipeline (marked + playwright)
- `C:/Users/RAS/projects/team-dashboard/.tmp/send_sup_tg.py` — sends PDF to TG via Rasclaw bot
- `C:/Users/RAS/projects/team-dashboard/src/data/aprilData.json` — source of truth; always query before writing numbers

## Open questions / blockers
- Does Fuse count toward the CMS productivity goal in Informdata's official scoring? RA confirming with supervisor. Answer determines whether Marielle = 54.0% or 43.2%

## In flight
- None
