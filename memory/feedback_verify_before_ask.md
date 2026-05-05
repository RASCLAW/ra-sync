---
name: Verify Before Asking RA For Manual Work
description: Always check local files/env/state before asking RA to fetch credentials, log into dashboards, or do manual work. Verify first.
type: feedback
related:
  - feedback_diagnostic_depth.md
  - feedback_mid_session_save.md
originSessionId: ec69ca92-4b44-470f-a2c1-88e55b8f2d58
---
**Rule:** Before asking RA to fetch a credential, open a dashboard, or do any manual lookup, grep/check local files for the thing first. If it's already there, skip the ask.

**Why:** Session 124 (2026-04-15) Cloud Run migration plan — I told RA to open Meta App Dashboard and copy `META_APP_SECRET` into `.env`. He was at work (network blocks dev domains), went to his locker to tether phone + ask Rasclaw bot for guidance. Rasclaw said "it's already in your files." I grepped and confirmed — `META_APP_SECRET` was already in `.env`, 32 chars, correct format. The whole ask was wasted effort + frustration because I didn't verify first.

**How to apply:**
- Before any `.env` / credential / config lookup instruction, grep for the key name first
- For secrets: `python -c "from dotenv import dotenv_values; print([k for k in dotenv_values('.env') if '<TERM>' in k.upper()])"` — shows key presence without leaking value
- For file state: Glob/Read the file before asking RA to find it
- For GCP/cloud state: try `gcloud ... describe` before asking RA to check console
- When writing plans/tasks that include "RA fetches X" — add a prior subtask "verify X isn't already present" with specific grep command
- Applies especially when RA is on limited network (work, mobile) — manual steps have higher friction cost

The principle: RA's time + attention is scarce. Every "can you go check X" that could've been answered by a 5-second grep is a defect in the assistant loop.
