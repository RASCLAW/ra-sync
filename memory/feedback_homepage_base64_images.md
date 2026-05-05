---
name: Homepage Base64 Images
description: index.html is 6.9MB with base64-embedded images -- cannot Read directly; use Python to extract
type: feedback
related: [project_dubery_v3_landing.md]
originSessionId: 4eb6ffb9-b3a9-475e-bde5-b5ff542047ea
---
`dubery-landing-v3/index.html` is ~6.9MB because images (series cards, hero shots, etc.) are embedded as base64 data URIs directly in the HTML. The Read tool hits token limits and cannot load it.

**Why:** The v3 editor embeds images as base64 when saving via the browser editor.

**How to apply:** When you need an image from the homepage (e.g. to reuse a series card image on another page), don't try to read index.html — use this Python extraction pattern instead:

```python
import re, base64
with open('dubery-landing-v3/index.html', 'r', encoding='utf-8', errors='ignore') as f:
    content = f.read()
pattern = r'class="series-card"[^>]*>.*?<img[^>]+src="(data:image/[^;]+;base64,([^"]+))"'
matches = re.findall(pattern, content, re.DOTALL)
for i, (full, b64) in enumerate(matches[:3]):
    with open(f'assets/hero/series-{i}-new.png', 'wb') as out:
        out.write(base64.b64decode(b64.strip()))
```

Save extracted images with a `-new` suffix to avoid overwriting any existing stale versions. Extracted copies: `series-bandits-new.png`, `series-outback-new.png`, `series-rasta-new.png` in `assets/hero/`.
