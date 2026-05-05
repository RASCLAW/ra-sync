---
name: CRM ADC Scope Issue — Use token.json Locally
description: ADC on RA's Windows has no Sheets scope; chatbot CRM must use token.json for local dev, ADC only on Cloud Run
type: feedback
related: [feedback_chatbot_architecture.md, project_chatbot_recovery_complete.md, feedback_google_api_cache_discovery.md]
originSessionId: 6301781d-2f22-4e59-b1f1-3a4187f641a5
---
Always gate ADC behind `os.environ.get("K_SERVICE")` in `crm_sync._get_service()`. Local dev always falls through to `token.json`.

**Why:** RA's Windows ADC is configured for Vertex AI / Veo tools only — it doesn't have the `spreadsheets` scope. When the chatbot tried ADC first (old code), it succeeded (returned credentials) but every Sheets API call returned HTTP 403 `ACCESS_TOKEN_SCOPE_INSUFFICIENT`. CRM silently failed on every message. The `K_SERVICE` env var is set by Cloud Run automatically, so the gate is zero-config for both environments.

**How to apply:** Any new Google Sheets integration on this machine must use `token.json` (OAuth2 user credentials), not ADC. ADC is for Vertex AI / Gemini / Veo only.
