---
name: googleapi httplib2 fallback to requests
description: When googleapiclient hangs/times out on sheets.googleapis.com, drop down to requests + bearer token from google.oauth2 -- httplib2 has a separate connection-stack issue from cache_discovery
type: reference
related: [feedback_google_api_cache_discovery.md, reference_dubery_orders_sheet.md, feedback_sheets_execute_timeout.md]
originSessionId: weekend-order-recovery-shipped
---

If `googleapiclient.discovery.build("sheets", ...).execute()` times out with `WinError 10060` while `curl https://sheets.googleapis.com` returns 404 instantly, the issue is httplib2 (the transport googleapiclient uses), not connectivity. Drop down to plain `requests` against the REST endpoint with a manually-pulled bearer token.

**Working pattern:**

```python
from google.oauth2.credentials import Credentials
from google.auth.transport.requests import Request
import requests

creds = Credentials.from_authorized_user_file('token.json')
if not creds.valid:
    creds.refresh(Request())

SHEET_ID = '...'
url = f'https://sheets.googleapis.com/v4/spreadsheets/{SHEET_ID}/values/{tab}'
r = requests.get(url, headers={'Authorization': f'Bearer {creds.token}'}, timeout=30)
data = r.json()
values = data.get('values', [])

# batchUpdate:
url = f'https://sheets.googleapis.com/v4/spreadsheets/{SHEET_ID}/values:batchUpdate'
body = {'valueInputOption': 'RAW', 'data': [{'range': 'Orders!L4', 'values': [['...']]}]}
r = requests.post(url, headers={'Authorization': f'Bearer {creds.token}', 'Content-Type': 'application/json'}, json=body, timeout=30)
```

**Why:** httplib2 (used internally by googleapiclient) has known issues on Windows with IPv6 / proxy detection / cert chain that can manifest as repeated 10060 timeouts even when the same host is reachable via curl. `requests` uses urllib3 + a separate connection stack that doesn't share the bug. The OAuth credentials object is transport-agnostic -- `creds.token` is a bearer token usable by any HTTP client.

**How to apply:** First retry once -- timeouts can be transient. If it fails 2x in a row but curl works, switch to the `requests` pattern above instead of debugging httplib2. Works for any Google REST API (Sheets, Drive, Gmail) since they all accept the same bearer token. Add Windows-cp1252-safe stdout rewrap (`sys.stdout = io.TextIOWrapper(sys.stdout.buffer, encoding='utf-8')`) if printing non-ASCII content.

**Distinct from [[feedback_google_api_cache_discovery]]:** that one was the discovery-document fetch hanging at `build()` time (fixed by NOT passing `cache_discovery=False` + pre-warming). This one is the actual API call hanging at `.execute()` -- a different layer of the same library.
