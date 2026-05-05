---
name: Python HTTP to Google APIs slow on Windows — IPv6 is the root cause
description: googleapiclient/httplib2 hang AND plain requests/urllib are ~60s slow. Root cause is IPv6 preference with no working IPv6 route. Fix: force IPv4 via socket.getaddrinfo monkey-patch.
type: feedback
related: [reference_vertex_ai.md, reference_token_scopes.md, feedback_content_storage_rule.md, reference_cloud_run_chatbot.md]
originSessionId: 3d75bacc-60e4-44d2-8b0f-716a6f44b55b
---
Python HTTP calls to Google APIs from RA's Windows machine are ~60× slower than `curl` because of an IPv6 resolution issue. **Fix: force IPv4 only by monkey-patching `socket.getaddrinfo` at the top of the module (before any HTTP imports).**

**Root cause (confirmed session 99):**
- `socket.getaddrinfo("aiplatform.googleapis.com", 443)` returns IPv6 addresses first (`2404:6800:4017:...`)
- RA's home ISP doesn't route IPv6
- Python's `socket.create_connection` tries IPv6 first, waits ~60s for the OS-level TCP timeout, THEN falls back to IPv4
- That's the 60-second-per-call latency
- `curl` prefers IPv4 by default on Windows, so it's ~1.5s

**Proof (session 99 chatbot refactor):**
- `curl` direct to Gemini REST: **1.4s**
- Python `requests` same endpoint: **87s**
- Python stdlib `urllib.request.urlopen`: **61s**
- 3 sequential `urllib` calls: 61s, 61s, 62s (consistent, not a cold-start cost)
- After IPv4 monkey-patch: **1.7s** (all 3 calls)

**The fix (drop this at the TOP of any module making HTTP calls to Google APIs):**
```python
import socket as _socket
_orig_getaddrinfo = _socket.getaddrinfo
def _ipv4_only_getaddrinfo(*args, **kwargs):
    return [r for r in _orig_getaddrinfo(*args, **kwargs) if r[0] == _socket.AF_INET]
_socket.getaddrinfo = _ipv4_only_getaddrinfo
```

Must be before `import requests`, `import urllib`, `import google.auth`, etc. — anything that caches the original `getaddrinfo`. Applies to the whole process.

**History of the same underlying problem:**
1. Session 97 Cloud Run: `google-genai` SDK hung during `Client()` init → fixed by direct REST to Vertex AI (Cloud Run is a different symptom — container networking)
2. Session 99 local (Drive API): `googleapiclient`/`httplib2` timed out on every call → originally blamed httplib2
3. Session 99 chatbot refactor: `requests` + `urllib` ALSO ~60s slow → proved the issue is NOT httplib2, it's IPv6

**How to apply:**
- Any new Python tool hitting Google APIs from RA's home PC must include the IPv4 monkey-patch OR use subprocess curl
- If a call hangs or is mysteriously slow, suspect IPv6 first, not the HTTP library
- The monkey-patch is process-wide but harmless — IPv4 routing works fine for all Google endpoints
- Reference implementation: `cloud-run/conversation_engine.py` (top of file)

**Note:** On Cloud Run / Hetzner / Oracle, IPv6 works fine. This patch is a no-op in those environments (it just filters a single-result list down to the same single result). Safe to include in code that runs both locally and in the cloud.

**If you'd rather use `googleapiclient` specifically:** the old tools using `googleapiclient.discovery.build()` + `.execute()` may still have latent issues because `httplib2` has its own socket handling. Direct REST via `requests` + the IPv4 patch is the clean path.
