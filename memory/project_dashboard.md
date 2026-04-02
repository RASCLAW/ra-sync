---
name: Life Dashboard State
description: ra-dashboard — family life dashboard on Vercel, current version and features
type: project
---

**What:** Personal family dashboard at ra-dashboard-lake.vercel.app. Tracks RA, Arabelle, Baby Jah — schedule, finances, health, todos, meals, alerts.

**Current version:** v4. Overview default landing, 4 profile views (overview/RA/Arabelle/Jah), pencil edit pattern, data pipeline rules.

**Key features:**
- Baby Jah tracker (fed/diaper/sleep/woke/activity)
- Finance snapshot (payroll, pocket cash, bills)
- Contextual status (reads sleep logs + trip context, not time-of-day guesses)
- Light/dark mode, CSS variable color system
- Quick Log sheet integration (pencil actions post to sheet + update UI via localStorage)
- Stale data detection (feeding gap >8h = red warning)

**Data:** Google Sheets (Dashboard Quick Log) → dashboard. No manual DB injection — structured data flows through pipeline only.

**Deploy:** Vercel. Multiple deploys per session typical.

**How to apply:** Dashboard UI work routes through moderator window. Use the dashboard agent for changes. Cash rules: pocket cash deducts on cash purchases, BPI balance only from screenshot uploads.
