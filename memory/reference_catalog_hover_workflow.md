---
name: Catalog Hover Image Update Workflow
description: How to replace catalog hover images using the catalog-edit HTML export + Python extraction script
type: reference
related: [project_dubery_v3_landing.md]
originSessionId: aa6a11ec-368a-4205-ae56-8a0d6fb1cadc
---
Use the catalog editor at `http://localhost:7070/products/?edit` to drag-drop replacement hover images, then save HTML. Extract the base64 images with this script:

```python
from pathlib import Path
import re, base64, json

html = Path("C:/Users/RAS/Downloads/catalog-edit (N).html").read_text(encoding='utf-8', errors='replace')
data = json.loads(Path("c:/Users/RAS/projects/DuberyMNL/dubery-landing-v3/products/data.json").read_text())
hover_files = [p["hover"].split("/")[-1] for p in data]
slugs = [p["slug"] for p in data]
matches = re.findall(r'<img class="bs-img hover"[^>]*src="([^"]+)"', html)
assets_dir = Path("c:/Users/RAS/projects/DuberyMNL/dubery-landing-v3/assets/catalog")

for slug, src, fname in zip(slugs, matches, hover_files):
    if src.startswith("data:image"):
        b64 = src.split(",", 1)[1]
        assets_dir.joinpath(fname).write_bytes(base64.b64decode(b64))
        print(f"saved {fname} ({len(b64)//1024}KB)")
```

**Verify drops registered:** Compare MD5 hashes between saves — identical hash = drop didn't register, try again.

**Why:** The catalog editor embeds replaced images as base64 data URLs directly in the HTML. The Python script extracts and writes them to the actual asset files.

**How to apply:** When RA wants to update hover or primary images on the catalog page without touching code. Run the server first: `cd dubery-landing-v3 && python -m http.server 7070 --bind 0.0.0.0`.

**Gotchas:**
- Drag-and-drop silently fails sometimes — always verify with hash check before assuming done
- Browser caches old images — use Ctrl+Shift+R after updating files
- Deleting asset files + hard refresh lets you see which cards still have old images
