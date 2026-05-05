---
name: Google Sheets execute() Has No Timeout
description: googleapiclient Sheets .execute() blocks indefinitely on SSL errors or network hiccups -- must background or wrap with timeout
type: feedback
related: [feedback_crm_adc_scope.md, feedback_google_api_cache_discovery.md]
originSessionId: 4a520b66-408c-4017-b042-95e3c5f75811
---
`service.spreadsheets().values().append(...).execute()` and `.get(...).execute()` have no built-in timeout. On SSL errors (`WRONG_VERSION_NUMBER`) or slow network, the call hangs forever, blocking the calling thread.

**Why:** Root cause of chatbot reply failures 2026-04-19. `crm_append_message` in the reply path was blocking the Gemini call. `crm_load_history` also blocking. Both had try/except but exceptions only catch raises -- hangs don't raise.

**How to apply:**
- All Sheets writes in the reply path must be fire-and-forget: `threading.Thread(target=fn, daemon=True).start()`
- Reads that block the reply (like `crm_load_history`) must use `concurrent.futures.ThreadPoolExecutor` with `fut.result(timeout=5)` and treat `TimeoutError` as empty result
- Never put a raw `.execute()` call inline in a latency-sensitive code path
