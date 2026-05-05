---
name: Vertex AI Rate Limits (Gemini 3.1 Flash image)
description: Effective ~2 parallel image gen calls before 429 RESOURCE_EXHAUSTED — batch 2 parallel with 25-30s stagger between waves
type: reference
related: [reference_vertex_ai.md, feedback_batch_ingest_pattern.md]
originSessionId: 2d508059-363a-4c95-9edb-672b753e5f69
---
Gemini 3.1 Flash image generation on Vertex AI caps effective concurrency at ~2 parallel requests. Higher bursts hit 429 RESOURCE_EXHAUSTED immediately. Diagnosed session 126 after firing 10 parallel → 3 succeeded + 7 failed.

## Observed behavior
- 10 parallel → 3 succeed, 7 fail with 429
- 5 parallel → same pattern (first 2-3 succeed, rest 429)
- 2 parallel + 25-30s stagger between waves → all succeed
- Default quota likely 10 RPM per model per user, but effective burst capacity is much lower (~3)

## Batch pattern that works
```python
WAVE_SIZE = 2
SLEEP_SEC = 25  # between waves
# ThreadPoolExecutor(max_workers=WAVE_SIZE) per wave, then sleep
```

For a 17-image batch at 2 parallel + 25s stagger = ~4 minutes wall time. Tolerable.

## Quota visibility
- 429 body does NOT include the quota name/limit — Google keeps it opaque
- Check quotas at https://console.cloud.google.com/iam-admin/quotas, filter by "Vertex AI" + "Generate image model requests per minute per base model per minute per user"
- Request quota increase if the 2-parallel cap blocks higher-volume work

## Session 126 runbook
- `.tmp/batch_gen_bank.py` — orchestrator that ran 14 remaining after initial burst fail. ThreadPoolExecutor(2) + 25s sleep pattern. Handles per-subprocess timeouts.
- After orchestrator, 1 individual retry at 60s post-wave resolved the last failure.

## RA's paid-API rule (feedback_build_quality.md)
Always stop on first 429 and report. Don't retry-storm. The 429s don't consume quota themselves, but re-firing a failing pattern wastes wall time.
