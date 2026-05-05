---
name: Informdata CRIM Dashboard Project
description: Tier 3 production-wide HTML dashboard for RA's day job productivity data — director-level audience, data analyst resigned
type: project
related: [user_work_role.md]
originSessionId: 17b4d415-67c9-470f-ba30-e30e77982b77
---
Self-contained HTML productivity dashboard for Informdata/Valor Global CRIM team (August 2025). Director + ops manager audience. Data analyst resigned — RA stepped in. **BUILD COMPLETE as of Session 152.**

**Why:** Data analyst resigned; RA offered to help with reporting. Career opportunity + portfolio piece.

**How to apply:** Dashboard is built. To regenerate: `python generate_report.py`. Agent productivity: `python generate_productivity_report.py`. Both scripts in `C:\Users\RAS\projects\informdata-data-analysis\`.

## Architecture
- `data.py` — loads Excel, computes all KPIs + aggregations, returns structured dict
- `template.py` — `build_html(month_year, data)` returns complete HTML; all rendering in JS
- `generate_report.py` — CLI entry point for main dashboard
- `generate_productivity_report.py` — CLI entry point for agent productivity standalone HTML

## Session 152 Final State (Savepoint 3)
- Quality KPI card shows avg accuracy % (unchanged); clicking it switches TM chart to **errors per TM** (red bars from RAW_QP)
- Downtime flipped to **Active Time** everywhere (`100 - downtime`): KPI card, leaderboard, Teams tab, TM chart
- Agent productivity standalone HTML built — `agent_productivity_august_2025.html`
- README.md added

## Agent Productivity Formula
- `PTG = Actual Orders ÷ Total Expected Units` (from MTD Source, pre-computed)
- `Orders/Day = Actual ÷ 21 working days`
- `Daily Goal = Total Expected Units ÷ 21`
- Uses `Total Expected Units` (not QA TAT or CMS hours) — handles CMS/Fuse platform split automatically
- See `feedback_qa_tat_not_hours.md` for why QA TAT Seconds cannot be used as hours proxy

## Dashboard Features (Session 152 Final)
- Filter slicers: TM multi-select, Region multi-select, Tier toggles (Green/Yellow/Red)
- All data renders client-side in JS — KPIs, charts, tables all update on filter change
- **KPI cards (clickable):** Productivity | Quality/Accuracy | Active Time | Total Orders
  - Quality click → TM chart shows errors per TM
  - Active Time click → TM chart shows avg active time % per TM
- **Team Performance per TM chart:** horizontal bar, one bar per TM, re-sorts and re-colors per selected KPI
- **Weekly Trend chart:** combo bar (Total Orders) + line (Avg PTG)
- Leaderboard: 151 rows, sort, search, CSV export, 9 columns (Downtime replaced by Active Time)
- Quality tab: error category chart + flagged processor table with CSV export
- Teams tab: TM summary (Active Time column, not Downtime)

## Data Quirks (verified)
- `Actual Work Time %` = `1 − Downtime %` exactly — perfect inverses, do not use both (see feedback_informdata_actual_work_time.md)
- Paycom ÷ Total Expected Hours Worked = constant ~1.176 (formula artifact, not real attendance signal)
- No true attendance/absenteeism metric exists in this Excel

## Data Sources (all in C:\Users\RAS\Downloads\)
| File | Sheet | Used For |
|------|-------|----------|
| August_2025 CRIM Productivity.xlsx | MTD Source | 151 processors, KPIs, leaderboard |
| August_2025 CRIM Productivity.xlsx | Weekly Source | Week 1–4 trend |
| August_2025 CRIM Productivity.xlsx | Quality | 13 error categories, QA audit log |
| August_2025 CRIM Productivity.xlsx | Regional Source | Region name lookup |

## Known Data Quirks
- MTD Source `Region` col = numeric — joined from Regional Source by name (3-tier fuzzy match)
- 5 processors with ñ in names can't resolve to named regions — show numeric code (by design)
- "Pacfic Northwest" typo in source Excel (missing i) — not a code issue
- Avg accuracy = 100.0% suspicious — Accuracy column may be near-1.0 floats; verify in Excel
- Weekly Source deduped by Name+Week using `max(Total Combined Orders)` per group

## August 2025 Numbers
- 151 processors, 13 TMs (after dedup), 6 named regions, 4 weeks, 13 error categories
- 128,388 total orders, 94.0% avg PTG, 68/151 above goal, 31 processors with quality errors
