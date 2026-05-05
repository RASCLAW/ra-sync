---
name: Team Faith Dashboard Project
description: React dashboard for April 2026 team productivity scorecard — source file, update workflow, and current feature state
type: project
related: [feedback_table_min_width_mobile.md, user_work_role.md]
originSessionId: 590dce33-e1ff-43a6-ad42-807fe9b70a39
---
Team Faith Dashboard is a React/Vite app at `C:/Users/RAS/projects/team-dashboard/` built to visualize the April 2026 CMS production scorecard for RA's Informdata team.

**Source file:** `April Team Faith.xlsx` (downloaded to Downloads/)
- **Goal Calculator** tab — Exp/Hr per jurisdiction (lookup table)
- **Agent tabs** — one per team member; daily rows (rows 19–138), jurisdiction summary (rows 145–155), totals (row 139), defects/accuracy (row 156)

**Update workflow:**
1. Download new Excel → `python extract.py` → regenerates `src/data/aprilData.json`
2. Restart dev server (`npm run dev` in `team-dashboard/`)

**Key formulas (all computed in processData.js, not extract.py):**
- Paycom hours = active CMS days × 8h (actual paycom not in Excel)
- totalExpected = dailyTarget × activeDays (dailyTarget = expHourlyUnits × 8)
- % to Goal = Total Completed ÷ totalExpected × 100
- Per-jurisdiction % to Goal = jurisdiction completed ÷ totalExpected × 100 (rows sum to agent total)
- Pass threshold: Productivity ≥ 85%, Quality ≥ 99.95%

**Session 6 features (2026-05-05):**
- Heatmap calendar in IndividualDashboard: 26px cells, Sun-first, date numbers inside cells, hover tooltips
- Sticky header on IndividualDashboard (z-index 40)
- Team Pulse card: SVG 270° GoalGauge + spring animation, below leaderboard
- Full tooltip coverage: CardTooltip in TeamDashboard, all MetricCards + section headers + leaderboard columns
- Compact leaderboard: icon-only PassBadge, px-2 padding, max-w-7xl, min-width:860px for mobile scroll
- Dynamic date range in disclaimer and chart titles (computed from agent data)
- Mobile fixes: `touch-action: pan-x pan-y`, MetricCard responsive padding, heatmap cell size reduced
- Chart + heatmap in separate cards, side-by-side via `grid-cols-[1fr_310px]`
- Jurisdiction table: Goal/hr column first, totalExpected in footer with tooltip, per-row % sums to total
- Section labels: "On Track" / "Needs Attention" / "Quality & Fuse Summary"
- **Deployed** — https://team-faith.pages.dev (Cloudflare Pages; vercel.app blocked at RA's work)

**Agents:** Apolo Jay Mombay, Gwenwyfar Oshea Deriada, Jhayson Banaga (inactive), Laurence Lazaro, Leo Paul Quining, Marielle Ganacan, Marjorie Montemayor (inactive), Mighan Gallego, Raclef Jay Dichoso, Ronald Adrian Sarinas

**Session 6 continuation (2026-05-05 savepoint 3):**
- DASHBOARD_REPORT.md — 11-section builder-facing analysis (data model, all JSON fields, formulas, limitations, scaling plan)
- SUPERVISOR_BRIEF.md — data-driven supervisor report; all numbers verified from aprilData.json before writing
- PDF pipeline: `marked` (MD→HTML) + playwright (HTML→PDF); scripts at `.tmp/make_sup_pdf.cjs` + `.tmp/send_sup_tg.py`; must use `.cjs` because package.json has `"type": "module"`
- Both PDFs sent to TG (Rasclaw bot, chat 1762124488)
- Open: Marielle Fuse scoring — 54.0% includes Fuse, 43.2% CMS-only; supervisor to confirm which applies

**Why:** Portfolio piece + practical tool. Shows RA can build data-driven dashboards from raw Excel.

**How to apply:** When RA mentions "team dashboard", "Team Faith", "Faith dashboard", "scorecard dashboard", or asks about the Informdata team's production data — this is the project.
