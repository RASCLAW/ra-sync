---
name: Cloud Run CPU throttling — big cost lever
description: Dropping --no-cpu-throttling on Cloud Run cuts costs ~85% with minimal risk if background threads are not in use.
type: reference
related: [reference_cloud_run_chatbot.md, project_chatbot_live.md]
originSessionId: d71c4cad-237e-497f-b80e-83b5efe31d27
---
Cloud Run's `--no-cpu-throttling` flag is the biggest cost driver for always-on services. Flipping it off saves ~85% of instance cost.

**Cost math (DuberyMNL chatbot, 1 vCPU + 512 MiB + min-instances=1):**

| Config | CPU billing | Est. monthly cost |
|---|---|---|
| `--no-cpu-throttling` | Full rate 24/7 | ~$47/mo CPU + $2.60/mo mem = **~$50/mo** |
| Default (throttled) | Idle rate (~10%) when no request | ~$4.65/mo CPU + $2.60/mo mem = **~$7–8/mo** |

**What throttling changes:**
- Instance stays warm (min-instances=1 still pins 1 instance)
- No cold starts on normal traffic
- CPU only billed at full rate during active requests
- Idle CPU billed at ~10% rate (not 0)
- In-memory state (caches, conversation stores) survives
- Startup CPU boost still works — warmup threads still complete

**What breaks:**
- Background threads that run *between* requests — die/stall
- Periodic cleanup loops, heartbeat pings, cache TTL reapers
- Daemon threads were already unreliable on Cloud Run regardless (session 97 lesson)

**Before flipping:** grep for `threading.Thread`, `Timer`, `schedule`, `apscheduler`, `BackgroundTask`, `asyncio` to confirm no background work exists.

**Switch commands:**
```bash
# Enable throttling (cheap mode)
gcloud run services update SERVICE --region=REGION --cpu-throttling

# Back to always-on (expensive mode)
gcloud run services update SERVICE --region=REGION --no-cpu-throttling
```

**Rollover cost:** ~2 × 504s during the ~20s revision change window (Meta retries these). First message after rollover warms slowly (30–60s) then stabilizes.

**DuberyMNL chatbot applied this in session 99 (2026-04-10), revision 00034-w5q.** Pre-existing Gemini latency spikes (10–18s) are unchanged — they're not caused by throttling.
