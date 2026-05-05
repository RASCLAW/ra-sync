---
name: Node Playwright When Python Broken
description: RA's Python playwright install has broken greenlet DLL (ImportError). Use `npx playwright@1.48.0` via Node instead — works out of the box, no install.
type: reference
related:
  - reference_playwright_gallery_scrape.md
---
On RA's Windows laptop, `from playwright.sync_api import sync_playwright` in Python 3.12 fails with `ImportError: DLL load failed while importing _greenlet`. Node playwright via `npx` works fine and has the same API surface for visual verification.

**Why:** Discovered during Marketing tab thumbnail debugging — needed to actually inspect DOM computed styles and screenshot the running Command Center. Python path was blocked; `npx --yes playwright@1.48.0` ran clean in ~30s first-run.

**How to apply:**
- For visual verification / screenshot / DOM inspection tasks, default to a tiny Node script:
  ```js
  const { chromium } = require('playwright');
  (async () => {
    const browser = await chromium.launch();
    const page = await browser.newPage({ viewport: { width: 1600, height: 1000 } });
    await page.goto(URL, { waitUntil: 'networkidle' });
    await page.screenshot({ path: 'out.png' });
    const info = await page.evaluate(() => ({ /* computed styles, etc */ }));
    console.log(JSON.stringify(info, null, 2));
    await browser.close();
  })();
  ```
- Run with `node .tmp/script.js`. First run of `npx playwright` downloads chromium (~200MB).
- Do NOT reach for python playwright in this repo until the greenlet DLL is fixed (probably needs `pip install --upgrade greenlet` with Visual C++ build tools).
