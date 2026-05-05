---
name: Cloud Run Chatbot Deployment
description: How to deploy a webhook chatbot on Cloud Run with Vertex AI Gemini -- gunicorn settings, CPU throttling, Meta timeouts, model names
type: reference
related:
  - reference_vertex_ai.md
  - reference_gcloud_cli.md
  - project_messenger_strategy.md
  - reference_cloud_run_cpu_throttle.md
  - project_oracle_migration.md
originSessionId: d71c4cad-237e-497f-b80e-83b5efe31d27
---
## Cloud Run + Webhook Chatbot Architecture

### Background Processing
- Cloud Run throttles CPU to near-zero after HTTP response is sent (default behavior)
- `--no-cpu-throttling` keeps CPU alive for background threads after response
- Even with no-cpu-throttling, idle instances can be killed anytime (SIGTERM) with min-instances=0
- SIGTERM gives 10 seconds before SIGKILL

### Gunicorn Settings (Critical)
- **`--timeout 0`** -- Google officially recommends this for Cloud Run. Default 30s kills workers before Cloud Run's own timeout.
- `--workers 1 --threads 8` is the standard recommendation
- Let Cloud Run's request timeout (`--timeout=60s` in deploy) be the sole enforcer

### Meta Webhook Requirements
- Must return 200 within **5 seconds** (not 20)
- After 15 min of failures: webhook disabled alert
- After 1 hour of failures: app unsubscribed entirely
- Retry behavior: immediate retries, then exponential backoff
- The 200 response just acknowledges receipt -- bot reply is sent separately via Send API
- **Pattern**: return 200 immediately, process in background thread, send reply via Send API

### Vertex AI Gemini Model Access
- Cloud Run default service account needs `roles/aiplatform.user` IAM role
- ADC works automatically on Cloud Run (metadata server provides credentials)
- No GOOGLE_APPLICATION_CREDENTIALS env var needed

### Model Names
- `gemini-2.5-flash` with `location="global"` -- current stable, recommended
- `gemini-2.0-flash` -- restricted to existing customers, sunset June 2026. Returns 404 for new projects.
- Always use `location="global"` for text generation (better availability, fewer 429s)

### Critical: Use REST API, not google-genai SDK
- `google-genai` SDK hangs on Cloud Run during client initialization
- Use direct REST API via `requests` + `google.auth` for ADC tokens
- Endpoint: `https://aiplatform.googleapis.com/v1/projects/{PROJECT}/locations/global/publishers/google/models/{MODEL}:generateContent`

### Critical: Messenger Send API needs PAGE token
- `META_ADS_ACCESS_TOKEN` does NOT work for `/me/messages` endpoint
- Must use `META_PAGE_ACCESS_TOKEN` for all Messenger Send API calls
- Both comment_responder.py and messenger_webhook.py should use PAGE token

### Critical: Synchronous processing, not background threads
- Daemon threads die silently on Cloud Run (even with --no-cpu-throttling + min-instances=1)
- Process webhook inline: receive -> Gemini -> send reply -> return 200
- Total ~3-4 seconds, within Meta's 5-second webhook timeout

### Deploy Settings (Recommended)
```
--no-cpu-throttling
--min-instances=0
--max-instances=3
--memory=512Mi (required with no-cpu-throttling)
--cpu=1
--timeout=60s
--concurrency=80
--port=8080
```

### Cost
- Free tier: 2M requests/month, 180K vCPU-seconds
- At 20 messages/day: effectively $0
- With --no-cpu-throttling (instance-based billing): pennies/month at this volume
