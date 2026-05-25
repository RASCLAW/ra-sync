---
name: vertex-quota-parallel-4-blows
description: Firing 4 parallel generate_vertex.py calls instantly hits Vertex Gemini 3.1 Flash image-preview 429 quota wall. Sequential WAVE_SIZE=1 + 2s inter-call sleep + 30s backoff on retry is the only safe pattern.
type: feedback
related:
  - feedback_vertex_prompt_json_format.md
---

When batch-generating images via `tools/image_gen/generate_vertex.py`, use sequential firing (one process at a time) with a small inter-call sleep and exponential backoff on 429. Parallelism of 4 or more = instant `RESOURCE_EXHAUSTED` after the first wave clears.

**Why:** Validated in session 170. First 56-image batch fired with `ThreadPoolExecutor(max_workers=4)` -- got 6/56 OK and 50/56 instant 429s. The first wave (4 parallel) cleared, then every subsequent wave got rejected pre-billing with `google.genai.errors.ClientError: 429 RESOURCE_EXHAUSTED`. Retry with `WAVE_SIZE=1` + `INTER_CALL_SLEEP=2.0s` + `RETRY_429_BACKOFF=30s` (3 retries max) on the same 56 prompts: 42/56 completed in ~30 min before manual stop; 54/56 on a follow-up batch in ~47 min. 429s are not billed (rejected pre-generation), so retries are free credit-wise but cost time.

**How to apply:**
- In batch orchestrators that spawn `generate_vertex.py`: `WAVE_SIZE = 1`, never 4+
- Add an `INTER_CALL_SLEEP` ~2s between calls to stay well under the per-minute quota
- On 429 / `RESOURCE_EXHAUSTED` in stderr, retry with 30s sleep, up to 3 times
- Reference orchestrator pattern in `.tmp/run_wf2_batch.py` (session 170) -- shows the working sequential + backoff loop
- ETA math: ~25-35s per Vertex call + 2s sleep + occasional 30s retries. 56 images ~= 30-45 min sequentially. Plan accordingly.
- Cost: ~$0.04 per successful generation. 56 images ~= $2.20. 429s are free.
