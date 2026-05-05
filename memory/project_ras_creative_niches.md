---
name: RAS Creative SOLUTIONS Niche Strategy
description: 6-niche vertical list, solar primary + battery pairing, strict DuberyMNL-complete gate, passive reading track during recovery window
type: project
related:
  - project_positioning_locked.md
  - project_portfolio_rebuild.md
  - project_valor_internal_pitch.md
  - project_chatbot_template_reusable.md
  - project_brand_pipeline.md
originSessionId: 3d75bacc-60e4-44d2-8b0f-716a6f44b55b
---
**Decided 2026-04-12** in strategic discussion with Claude (no code written — pure strategy lock). Downstream from `project_positioning_locked.md`.

## The 6-Niche List (prioritized)

1. **Solar panel installers** — PRIMARY. RA personally interested, wants to learn solar + battery tech. Near-zero agency competition. Highest ticket (P200K-P2M per install). Email-first B2C + B2B. Fallout leads moat maps perfectly to tire-kicker filtering at high volume. Long sales cycle makes chatbot + nurture retainer obviously valuable.
2. **Tour operators** — UGC + chatbot synergy play. DuberyMNL's `/dubery-ugc-pipeline` + ad creative skills port cleanly (same visual-first, experiential, Messenger-native DNA). High-ticket bookings (P15K-P80K). Zero agency competition. Visual portfolio gold.
3. **Wedding photographers / videographers** — High ticket (P50K-P200K per wedding). Strong visual proof story. But creative types resist "systems" framing — pitch cautiously.
4. **Real estate brokers** — Safest financial pick (P50K-P500K commissions justify any retainer). Highest pitch fatigue in this list. Best for small brokerages (5-20 agents) and solo top-producers.
5. **Immigration / visa consultants** — Email-first, near-zero competition, high-anxiety buyers need fast replies, moat fit strong. P10K-P80K per case.
6. **Car detailing / PPF / ceramic coating** — Messenger-native opportunistic pick. Lower ticket but higher volume, appointment-based. Good for lower-friction retainer tests.

## Dropped from consideration

- **Dental / spa** — RA explicitly avoids; saturated at the pitch layer globally, heavy pitch fatigue
- **Review centers** — RA said "out of my league"
- **Home services (generic)** — owner-operator problem + retainer math too tight. Only viable if narrowly targeted (e.g., aircon cleaning companies with 5-10 techs)

## Sequencing (strict gate)

**Gate:** DuberyMNL MUST be complete before any RAS Creative SOLUTIONS build begins. This is RA's call, not a soft preference.

**"DuberyMNL complete" = all 9 recovery steps done:**
1. Image bank expansion 21 → 35-40 with per-image captions
2. duberymnl.com zone migrated Namecheap → Cloudflare (+ Email Routing if needed)
3. cloudflared named tunnel live at chatbot.duberymnl.com
4. Meta webhook re-wired to chatbot.duberymnl.com/webhook
5. Flask + cloudflared auto-start on PC logon
6. UptimeRobot monitoring
7. CRM test-data cleanup
8. Boosted ads unpaused
9. **1 week of clean production data captured for case study screenshots** ← real gate

Steps 1-8 are maybe 2-3 active sessions. Step 9 is a full week of *waiting* — the window where passive reading track runs.

**Post-gate sequence:**
- Solar niche launches FIRST
- Second parallel niche runs ONCE the solar template is live
- Remaining niches fork from solar template sequentially

## Second Niche = Battery Storage (not really "parallel")

Obvious pairing because it's effectively ONE niche with two pitch angles:

- **Same customer.** Most PH solar installers already sell or want to sell battery storage as upsell.
- **Same sales flow.** Quote-driven, technical, email-first, long consideration cycle, high-ticket, residential + commercial.
- **Same knowledge base.** Solar study = battery chemistry + inverter sizing + net metering + grid-tied vs hybrid. One curriculum.
- **Same acquisition pain.** Battery buyers are high-intent + high-anxiety (research for weeks). Perfect chatbot + nurture fit.
- **Zero forking cost.** Solar outreach template works verbatim — swap "solar ROI" for "battery ROI" in personalization.

**Frame as:** "PH clean energy installers" = one market, two product lines.

## Solar Scope

- **Both residential + commercial.** Automation handles both drafting angles.
- **Both PH + international.** Cold email PH primary. International via other platforms if cold email doesn't land (Upwork, LinkedIn, industry forums).
- **Email-first primary.** Messenger secondary (residential homeowners may DM, commercial procurement is email).

## Why Solar Is the Strongest Primary

- **Passion compounds.** RA wants to learn solar + batteries. Every hour of tech study = moat nobody else can build. Generic AI agencies don't know what string inverters, LFP chemistry, net metering, or SM-DSM grid charges are.
- **Highest ticket per deal.** One install = 12-24 months of retainer paid. Math is unbeatable.
- **Growing market.** Meralco rates climbing, grid reliability degrading, battery storage boom, rooftop solar still under 2% penetration in PH.
- **Zero competition.** No Filipino AI agency targets solar.
- **Moat fit is 1:1.** Installers get 200+ "how much po" tire-kicker inquiries per serious buyer. That's literally the Google fallout leads problem.

## Workflow Vision (same workflow = product + sales engine)

**Research → source leads → personalize emails → send pitch.**

This IS the deliverable AND the way RAS Creative SOLUTIONS gets its first clients. The flywheel:

```
Outreach engine lands solar client
    ↓
Same engine becomes their lead qualification system
    ↓
Their live data becomes the case study
    ↓
Stronger outreach → more solar clients
    ↓
Fork template to niche #2 (battery → tour → weddings → etc.)
```

Build ONE system with swappable prompt configurations, not 6 systems.

## Passive Reading Track (during DuberyMNL recovery window)

Zero-cost, zero-distraction, compounds while DuberyMNL runs. Target: ~30 min/day idle reading.

1. **Read.** Ram Mendoza, Solaric, Buskowitz, Freedom Solar — their websites, blogs, FAQs, reviews.
2. **Lurk.** PH solar/battery FB groups ("Philippines Solar Enthusiasts", "Solar Panel Philippines"). Read homeowner objections.
3. **Collect.** Screenshots of slow-reply complaints from FB comments — pitch ammunition.
4. **Bookmark.** Technical articles on string inverters, net metering, LFP vs NMC, grid-tied vs hybrid.
5. **Brand research.** Google: Solis, Sungrow, Deye, Huawei, BYD, Pylontech, Dyness + "review". Learn brand reputations.

By DuberyMNL step 9 (1 week data capture window), RA will know more about PH solar than 90% of "AI agency" pitchers.

## How to Apply

- **If RA proposes starting RAS Creative SOLUTIONS work before DuberyMNL step 9 is complete:** push back hard and point to this file. The gate is strict.
- **If RA asks "which niche first":** solar. Don't re-litigate. The reasoning is in this file.
- **If RA considers a niche not on this list (e.g., dental, spa, review centers):** they were explicitly dropped — ask if he's changing the strategy or just exploring.
- **If RA wants to parallelize beyond 2 niches:** push back. Sequential fork-from-template is the plan; true parallel dilutes personalization.
- **During DuberyMNL recovery, surface the passive reading track** when RA has idle time but doesn't want to context-switch into project work.

## Do Not Drift

- Solar is primary. Not "one of several options."
- Battery storage pairs with solar, not as a separate niche.
- Gate on DuberyMNL complete is strict, not soft.
- Workflow = product. Don't build custom infrastructure per niche — fork one template.
