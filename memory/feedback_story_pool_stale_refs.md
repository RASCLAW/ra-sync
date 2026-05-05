---
name: Story Pool Stale Refs
description: fb-stories-pool JSON can silently contain paths not committed to git; story_rotation.py dies hard with exit 1, no skip fallback
type: feedback
related: [project_story_rotation.md]
originSessionId: 403031e3-ac94-4d66-aeae-dc6953510459
---
`story_rotation.py` does a hard `open(image_path, "rb")` with no fallback — if a path in the pool JSON doesn't exist on the runner, it exits 1 immediately.

**Why:** Discovered 2026-04-25 when GH Actions run 24905669104 failed at slot 41/74. Four files referenced in `fb-stories-pool-2026-04.json` had been deleted locally but the JSON was never updated.

**How to apply:** After any image cleanup/deletion session, run a quick check before committing:
```python
python -c "
import json, os
data = json.load(open('contents/assets/fb-stories-pool-2026-04.json'))
missing = [p['path'] for p in data['picks'] if not os.path.exists(p.get('path',''))]
print(len(missing), 'missing:', missing)
"
```
If missing > 0, remove them from picks + update count, then commit. Proper fix (Drive/R2 runtime fetch) is on backlog.
