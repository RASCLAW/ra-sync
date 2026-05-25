---
name: feedback_google_oauth_revoke
description: Google OAuth refresh token revokes after ~6 months inactivity for non-verified apps; re-auth via InstalledAppFlow
metadata: 
  node_type: memory
  type: feedback
  related: 
    - feedback_crm_adc_scope.md
    - feedback_google_api_cache_discovery.md
  originSessionId: 20a9afa9-b6a1-4124-b19a-0fa178f77a98
---

`token.json` refresh token gets revoked by Google after ~6 months of inactivity (non-verified OAuth apps). When it happens:
- Error: `invalid_grant: Token has been expired or revoked`
- `creds.refresh(Request())` fails even with a valid refresh_token present
- CRM sync silently skips all writes

**Fix:** Re-run InstalledAppFlow manually:
```python
from google_auth_oauthlib.flow import InstalledAppFlow
SCOPES = ['https://www.googleapis.com/auth/spreadsheets']
flow = InstalledAppFlow.from_client_secrets_file('credentials.json', SCOPES)
creds = flow.run_local_server(port=0)
with open('token.json', 'w') as f:
    f.write(creds.to_json())
```
Run from `c:\Users\RAS\projects\DuberyMNL\`. Opens browser for Google login.

**How to apply:** If CRM sync logs `invalid_grant` or `Token has been expired or revoked`, run the above. Happens roughly every 6 months. Last re-auth: 2026-05-15.
