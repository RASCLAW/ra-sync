---
name: PDF Page Break Control
description: CSS + HTML pattern to prevent content cuts across pages in playwright-generated PDFs
type: feedback
related: [reference_cloudflare_pages_deploy.md, project_team_faith_dashboard.md]
originSessionId: bdfc8809-ed9c-49f3-ba17-93036b6b3ba8
---
Use this pattern for professional PDFs where tables, headings, and sections must not split mid-page.

**Why:** Default browser print rendering splits tables mid-row, orphans headings at page bottoms, and cuts code blocks — looks unprofessional for reports shared with supervisors or clients.

**How to apply:**

CSS rules to add:
```css
.report-section { break-inside: avoid; page-break-inside: avoid; }
h2, h3          { break-after: avoid; page-break-after: avoid; }
table, tr, pre, blockquote, li { break-inside: avoid; page-break-inside: avoid; }
```

HTML structure — wrap each h2 + its content in a `<section class="report-section">` so the heading glues to its first paragraph. Since `marked` generates flat HTML, do it in JS post-processing:
```js
rawHtml = rawHtml
  .replace(/(<h2[^>]*>)/g, '</section><section class="report-section">$1');
rawHtml = '<section class="report-section">' + rawHtml + '</section>';
rawHtml = rawHtml.replace('<section class="report-section"></section>', '');
```

Note: large tables that exceed one page will still need to span pages — `break-inside: avoid` pushes them to start on a fresh page but can't shrink them. That's acceptable behavior.
