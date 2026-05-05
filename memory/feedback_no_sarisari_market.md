---
name: Drop Heavy-PH-Flavor Scenes
description: RA dislikes sari-sari, open-air market, jeepney street, and rice paddy locations in the UGC randomizer
type: feedback
related: [feedback_no_jungle_shots.md, feedback_ra_aesthetic_preference.md, feedback_narrow_scenarios.md]
originSessionId: 7851c43f-0662-4ff6-9139-2fd2802275d3
---
Drop sari-sari store (loc#33), open-air market (loc#29), Manila jeepney street (loc#7), and rice paddy field edge (loc#26) from the randomizer's `LOCATIONS_PERSON` bank.

**Why:** Session 122 -- RA removed these in two passes. Sari-sari + market first, then jeepney street + rice paddy. All read as too local/market-aesthetic and conflict with the clean premium DuberyMNL direction RA prefers (clean premium > gritty/weathered, see `feedback_ra_aesthetic_preference.md`).

**How to apply:**
- All four removed from `tools/image_gen/v3_randomizer.py`. PH-flavored locations that stayed: Manila BGC rooftop bar (#6), Banaue rice terrace overlook (#13), countryside dirt road with carabao (#31). Those feel more premium/scenic vs gritty street/market.
