---
name: Portfolio Deploy Pipeline
description: How to deploy portfolio.html (193MB raw) to Vercel -- extract base64, copy assets, compress PNGs, redeploy
type: reference
related: [reference_portfolio_url.md, project_portfolio_rebuild.md]
originSessionId: cad9a5a4-e57d-4a4b-a925-d9b27617bd15
---
Source of truth: `c:/Users/RAS/projects/DuberyMNL/portfolio.html`
Deploy target: `c:/Users/RAS/projects/ras-portfolio/portfolio.html`
Live URL: https://ras-portfolio-one.vercel.app/portfolio.html
Pipeline script: `c:/Users/RAS/projects/DuberyMNL/.tmp/portfolio_deploy.py`

## Why a pipeline is needed
portfolio.html embeds all images as base64 blobs (~100 images, ~1.5MB each) making the raw file 193MB. Both GitHub Pages and Vercel have a 100MB file limit. The pipeline extracts blobs to files + compresses PNG→JPEG at q82, reducing total deploy to ~33MB.

## Deploy flow (run every time portfolio.html is updated)
```
cp "c:/Users/RAS/projects/DuberyMNL/portfolio.html" "c:/Users/RAS/projects/ras-portfolio/portfolio.html"
python3 c:/Users/RAS/projects/DuberyMNL/.tmp/portfolio_deploy.py
cd c:/Users/RAS/projects/ras-portfolio && vercel --prod --yes
```

## What portfolio_deploy.py does
1. Extracts all `data:image/...;base64,...` blobs → `assets/portfolio-images/img-NNN.{png,jpg}`
2. Copies all DuberyMNL-relative `src=` paths (contents/, dubery-landing/, references/) into ras-portfolio
3. Converts all PNGs → JPEG at q82 across all content dirs
4. Updates all HTML src references to match renamed files
5. Writes `errors='replace'` to handle Unicode arrows (→) on Windows cp1252

## Gotcha
Do NOT edit ras-portfolio/portfolio.html directly -- it gets overwritten on every deploy. Always edit DuberyMNL/portfolio.html.

## PDF export
For Upwork attachments or offline sharing, use the screenshot-stitch approach (not Playwright print-to-PDF which reflows layout). See `reference_portfolio_pdf_screenshot_stitch.md`.
Output: `ras-portfolio/portfolio_upwork.pdf` (1.9MB, 6 pages, pixel-perfect).
