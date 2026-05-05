---
name: gcloud CLI Setup
description: Google Cloud CLI authenticated, ADC configured, unlocks Vertex AI + Veo + Cloud Run
type: reference
related: [project_veo_video_gen.md, reference_vertex_ai.md, reference_youtube_api.md]
---

## Setup (completed session 90, 2026-04-08)
- gcloud SDK v563.0.0 installed at `C:\Users\RAS\AppData\Local\Google\Cloud SDK\`
- Authenticated: ronaldadriansarinas@gmail.com
- Project: dubery
- ADC credentials stored by gcloud (no manual token management)

## Auth Commands
- `gcloud auth login` -- CLI auth (use `--no-launch-browser` on Git Bash if redirect fails)
- `gcloud auth application-default login` -- ADC for Python SDK
- `gcloud auth list` -- verify auth

## Key Quirk
- Git Bash on Windows: browser OAuth redirect doesn't always reach CLI
- Fix: use `--no-launch-browser` and paste auth code manually

## What It Unlocks
- Vertex AI (image gen, video gen) without API keys
- Veo 3.1 video generation with speech
- Cloud Run, Cloud Functions, Cloud Scheduler
- BigQuery, Cloud Storage
- All via ADC -- no API key management needed
