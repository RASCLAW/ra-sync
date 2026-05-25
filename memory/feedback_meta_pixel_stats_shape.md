---
name: feedback-meta-pixel-stats-shape
description: Meta /{pixel_id}/stats?aggregation=event returns nested bins with a data array of {value, count} per bin, NOT a value dict; first parse attempt assumed wrong shape and silently aggregated 0 events
metadata:
  type: feedback
related:
  - project_cc_marketing_tab_v2
  - feedback_meta_requests_comma_encoding
  - feedback_meta_subcode_99_transient
---

When pulling Pixel funnel events via `GET https://graph.facebook.com/v21.0/{pixel_id}/stats?aggregation=event&start_time=X&end_time=Y`, the response is hourly-binned with nested event arrays:

```json
{"data": [
  {"start_time": "2026-05-20T07:00:00+0800",
   "aggregation": "event",
   "data": [
     {"value": "PageView", "count": 24},
     {"value": "Purchase", "count": 2},
     {"value": "ViewContent", "count": 2},
     {"value": "AddToCart", "count": 1}
   ]},
  ...one bin per hour in the window...
]}
```

Each bin has a NESTED `data` array of `{value, count}` records. NOT a flat `value: {Name: N}` dict.

**Why this hurts:** First implementation of `tools/meta_ads/pull_pixel_stats.py` assumed `value` was a dict and silently aggregated nothing — exit code 0, "Saved 0 pixel events" printed cheerfully. No error surfaced because the parser just skipped non-dict values. Took one debug round (raw curl into Meta) to spot the actual shape.

**How to apply:** Whenever a Meta endpoint returns hourly/daily binned data, expect nested arrays per bin and log/print the first raw payload during initial implementation. The parser that works:

```python
events = {}
for bin_row in payload.get("data", []):
    for ev in (bin_row.get("data") or []):
        ev_name = ev.get("value")
        if not ev_name:
            continue
        try:
            events[ev_name] = events.get(ev_name, 0) + int(ev.get("count", 0))
        except (TypeError, ValueError):
            continue
```

**Token scope:** Same `META_ADS_ACCESS_TOKEN` that powers `pull_insights.py` works for `/{pixel_id}/stats` — no extra permission requested. Pixel ID env var lives at `META_PIXEL_ID` (default `1513349880261420` for DuberyMNL v3 site).

**Don't auto-retry on failure** (per global tool-development rule for paid Meta APIs). Surface stderr clearly and let RA decide.
