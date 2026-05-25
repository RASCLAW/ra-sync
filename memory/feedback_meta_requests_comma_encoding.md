---
name: feedback-meta-requests-comma-encoding
description: Meta Graph API rejects URL-encoded commas (%2C) in the metric param -- must pass literal commas
type: feedback
related: [project_cc_crm_tab.md, project_meta_catalog.md]
---

Use `urlencode(..., safe=',')` when building Meta API URLs with comma-separated metric lists via Python `requests`.

**Why:** `requests.get(url, params={...})` encodes commas as `%2C`. Meta's Graph API `/insights` endpoint returns `(#100) The value must be a valid insights metric` when it receives `metric=a%2Cb%2Cc` instead of `metric=a,b,c`. The bug is silent -- same error as a genuinely invalid metric name, so easy to misdiagnose.

**How to apply:** For any Meta API call that takes a comma-separated list in a query param (metric, fields with multiple values that may have issues), build the query string manually:
```python
from urllib.parse import urlencode
qs = urlencode({"metric": ",".join(metrics), "access_token": token, ...}, safe=",")
r = requests.get(f"{base_url}?{qs}", timeout=10)
```
