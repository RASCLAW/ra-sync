---
name: order-tg-recovery-milestone
description: "2026-05-19 milestone -- fixed missed-order TG ping (Apps Script sendSms bug short-circuiting notifyTelegram), recovered Jeff Pisec P998, validated v3 website as a real conversion channel"
metadata: 
  node_type: memory
  related: 
    - reference_dubery_orders_sheet
    - project_first_closed_order
    - project_dubery_v3_landing
    - project_messenger_strategy
    - reference_googleapi_httplib2_fallback
  type: project
  originSessionId: 5d02208e-3cb6-4ba2-9c61-98a2cc1cccaf
---

# Order TG Recovery + v3 Website Channel Validation -- 2026-05-19

## What happened
4 real orders landed in the DuberyMNL Orders sheet over the weekend of May 14-16 via the v3 site's order form. None of them fired a Telegram notification despite the code being in place. RA only realized the gap on May 19 when checking the sheet manually.

## Root cause
The Apps Script `doPost` was calling an undefined `sendSms(data)` function (Semaphore SMS was scaffolded with a `SEMAPHORE_API_KEY` constant but the function body was never written). `sendSms is not defined` threw a ReferenceError, the outer `try/catch` swallowed it as `Logger.log('Error: ...')`, and `notifyTelegram(data)` -- the very next line -- never ran. Execution log showed "Completed" for every weekend run, masking the failure.

## Fix
- Removed the unwritten `sendSms(data)` call (commented out for when Semaphore gets wired later).
- Split each side-effect into its own `try/catch` in `doPost` so a failure in one can't block the others.
- Added `muteHttpExceptions: true` to the Telegram fetch.
- Enriched TG message with `caption_id` (order_form vs. PDP slug) + delivery fee + express flag.
- Added a `testTg()` smoke function callable from the editor (not via the webhook).
- Redeployed as new version of the bound webhook (Apps Script URL stayed the same).

## Recovered revenue
- **Jeff Pisec -- P998** (Bandits Green + Bandits Blue). **Picked up 2026-05-20 12:11 PM** from 115 I. Sanchez, en route to Mira-Nila Homes, Quezon City (Pay Cash). Direct result of fixing the bug + same-day outreach.
- **Sean Anton Reyes -- P1,497** (Bandits Green + Outback Blue + Rasta Red). **Picked up 2026-05-20 12:01 PM** from 115 I. Sanchez, en route to 205 Banawe, Muntinlupa (Pay Cash). Largest weekend order -- recovery closed.
- Mark Malenab's 5/14 order was already delivered through other channels (not actually missed via this gap).
- Apollo Planas cancelled (changed mind).

**Total weekend recovery confirmed out for delivery 2026-05-20: P2,495** across 2 COD orders both from the v3 site's order form.

## Strategic insight: v3 website is a real conversion channel
Pre-this-session assumption: the v3 site was a brochure + secondary close path; the chatbot Messenger funnel was the primary engine. The weekend gap inadvertently proved otherwise.

- **4 self-serve orders in 3 days**, zero chatbot interaction, zero RA manual close
- **Average AOV via order_form: ~P923** (598 + 598 + 1497 + 998) -- significantly higher than typical Messenger close (P598 single pair)
- People found the site, trusted it, self-served checkout end-to-end

**How to apply:** justify running a parallel Website-objective ad campaign alongside the Messenger funnel. Each captures a different buyer segment -- decisive self-server (higher AOV) vs. trust-builder DM (higher volume, lower AOV). Don't kill Messenger -- chatbot is the RAS Creative SOLUTIONS portfolio differentiator -- but reframe website as the scale lever.

## Why this matters for [[project_first_closed_order]]
First closed order (2026-04-15) validated the *hybrid* architecture under failure -- chatbot caught a stale price in an image. This recovery validates the *resilient data architecture* under a different failure -- notification pipeline broke for 5 days, but order data was safely in the sheet the whole time. Once the ping got fixed, the close was a 30-second DM. Both stories go into the RAS Creative portfolio: "even when one layer fails, the data integrity holds and recovery is straightforward."
