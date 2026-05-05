---
name: Portfolio PDF via Screenshot Stitch
description: fullPage Playwright screenshot → Pillow resize+slice → A4 PDF. Pixel-perfect, 1.9MB, beats Playwright print-to-PDF for layout fidelity.
type: reference
related: [reference_portfolio_deploy_pipeline.md, reference_node_playwright_fallback.md]
originSessionId: daf5df86-69cc-460d-a82c-99b35ba6bf10
---
Playwright `print-to-PDF` reflows HTML and distorts layout. The correct approach is a full-page screenshot stitched into A4 pages.

**Pattern:**

```python
# Step 1: Playwright full-page screenshot (serve from localhost to avoid canvas taint)
# node -e: goto http://localhost:9191/portfolio.html, fullPage: true, deviceScaleFactor: 2

# Step 2: Pillow stitch
from PIL import Image
import math

img = Image.open('portfolio_full.png')
target_w = 1240  # A4 at 150dpi
scale = target_w / img.width
img = img.resize((target_w, int(img.height * scale)), Image.LANCZOS)

page_h = 1754  # A4 height at 150dpi
pages = []
for i in range(math.ceil(img.height / page_h)):
    page = Image.new('RGB', (target_w, page_h), (255, 255, 255))
    slice_ = img.crop((0, i*page_h, target_w, min((i+1)*page_h, img.height))).convert('RGB')
    page.paste(slice_, (0, 0))
    pages.append(page)

pages[0].save('output.pdf', save_all=True, append_images=pages[1:], resolution=150, quality=90)
```

**Result:** 1.9MB, 6 pages, pixel-perfect.

**Why canvas taint matters:** Serving from `file://` taints canvas elements — JS image compression fails with SecurityError. Always `python3 -m http.server` + `localhost` URL before screenshotting.

**How to apply:** Any time a portfolio or web page needs to be exported as a PDF that must match the browser rendering exactly.
