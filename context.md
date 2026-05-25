# RA Context Sync
Last updated: 2026-05-25 PHT

## Who Is RA
- Ronald Adrian Sarinas, Manila, Philippines (PHT, UTC+8)
- Home: San Joaquin, Pasig
- Partner: Arabelle (Gmail: morenopabicoara@gmail.com, nickname: Maganda)
- Baby: Baby Jah (born June 24, 2024 -- ~23 months)
- Cat: Buki
- Business: DuberyMNL (polarized sunglasses, P499/pair, live e-commerce) + RAS Creative SOLUTIONS (AI automation service brand, solar installer niche, gated on DuberyMNL portfolio milestone)
- Day job: Informdata, night shift 8 PM - 5 AM, CMS processor (Team Maan, CMS ID 6736)

## Available Files
Only fetch these when relevant to the conversation -- not all at once:
- schedule.md, calendar.md, family.md, health.md, finances.md, vehicles.md, home.md, pets.md, plans.md, lifestyle.md, skillset.md

## Last 3 Days

### 2026-05-25 (Sunday) -- Session 174
**CC dashboard overhaul + Marketing tab v2.**
- CRM tab production-ready: 5 tiles (Leads, Orders, Revenue, 24h Orders, Units Sold 30d) + click-detail modal + Page Analytics tiles live (Reach 283K / Engagements 8.7K / Page Views 3.9K over 28d)
- Schedule tab: queue cards click-to-detail with FB-styled preview + Edit upcoming flow + 4-card column collapse + zoom slider
- AI Suggest agent rewritten as a thinking skill (READ IMAGE -> MATCH REGISTER -> 5-8 OPTIONS with image-emerging labels). Chat history persists across CC restarts.
- Marketing tab v2 SHIPPED: full analytics rewrite -- snapshot strip / adsets / sortable ads leaderboard with creative thumbnails / Pixel funnel + gap callout / 14d SVG trend / Page Analytics + Needs Attention split. Two new pullers (live_meta + pixel_stats). No mutations from UI.
- Cloudflare Worker first-touch gate live (silence active conversations when laptop is asleep)
- Meta Page Access Token rotated to 13 scopes incl. `read_insights`
- Drive sync_folder.py refactored (two-phase: batch existence + parallel uploads, ~10x speedup)
- /sendit refactored to be config-driven via `~/.claude/managed-syncs.json` + `managed-repos.json`

### 2026-05-24 (Saturday) -- Session 173
**Memory lint + closeout nudge.**
- /lint-memory run (first in 44 days). Split MEMORY.md into 4 sub-indexes, archived 9 stale files, indexed 4 useful orphans, cleaned 3 broken cross-refs.
- Closeout skill gained memory health check (nudges /lint-memory if files >70 or last lint >14 days)
- Orders consolidation: chatbot + CC all read/write the v3 DuberyMNL Orders sheet now (CRM > Orders tab deprecated)

### 2026-05-23 (Friday) -- Session 172
**Scheduler handoff plan.**
- Diagnosed late scheduled post: Task Scheduler power policy skips ticks on battery
- Drafted Meta-native scheduling handoff plan (14 tasks, queue-time handoff with cron as retry/verify safety net)

## Current Status
- **DuberyMNL chatbot LIVE** (Messenger via cloudflared tunnel + v3 site at duberymnl.com + Worker first-touch gate fallback when laptop sleeps)
- **Command Center fully shipped**: Home, CRM, Schedule, Marketing v2, Image Bank, Content Gen, Inventory, Monitor, Video tabs all working (`cc.duberymnl.com`)
- **Sales (last 7 days):** 3 closed orders, ~P3K revenue. All-time recent: 7 orders / P5,737 / 11 units.
- **Ads:** 2 active adsets, P70/day each (Bespoke UGC + Brand Graphics). P882 spend / 251 LPV / 4 messages / 1 Pixel purchase last 7d.
- **TG bots:** Rasclaw active for ops pings. Belle DORMANT (not running).
- **Backups:** Daily Drive sync of contents/new + contents/ready + ra-sync/memory. Secrets pinned `keepForever` on Drive.

## Pending / Next Up (top of mind)
- **TOP PRIORITY:** finish 1 week of clean DuberyMNL production data, then unblock RAS Creative SOLUTIONS build
- Tag live Meta ads with `ref=` values so chatbot's Phase 2 openers actually fire
- Upload kraft hero product-only shots to CDN for chatbot's 2-image combo
- RAS Creative: post-gate, reprice retainer + portfolio hero + cold outreach (solar installers primary niche; 2A Optical MY as eyewear prospect #1)
- BIR/DTI: downgraded to upgrade-path (GoGo Xpress accepts COD without it; revisit when invoicing or Flash/J&T needed)
- ra-sync repo slated for eventual archive once memories relocated (low urgency)

## Recent Quick-Look Numbers
- DuberyMNL Pixel funnel (7d): PageView 1309 -> ViewContent 137 (10.5%) -> AddToCart 9 (0.7%) -> Purchase 4 (0.3%)
- FB Page: 2,098 followers, +27 this week, 28d reach 283K
- Top ad: bespoke-outback-red-graphic-a at P2.01/LPV
