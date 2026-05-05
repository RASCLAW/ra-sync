---
name: Cloudflare Pages Deploy (wrangler CLI)
description: Deploy a static build to Cloudflare Pages via npx wrangler; production URL requires --branch main flag
type: reference
related: [project_team_faith_dashboard.md, feedback_touch_action_mobile_scroll.md]
originSessionId: 70cb860f-dce5-459b-b08d-0b4176db170a
---
Deploy a pre-built `dist/` folder to Cloudflare Pages without installing wrangler globally.

**Create project (first time only):**
```bash
npx wrangler pages project create <project-name> --production-branch main
```

**Deploy to production:**
```bash
npx wrangler pages deploy dist --project-name <project-name> --branch main --commit-dirty=true
```

- `--branch main` is required — without it, wrangler deploys to a preview URL (`<hash>.<project>.pages.dev`), not the root production URL (`<project>.pages.dev`)
- `--commit-dirty=true` suppresses the warning about uncommitted git changes
- Files already uploaded are skipped (incremental upload)

**Why:** Discovered on Team Faith Dashboard — first deploy showed "Nothing is here yet" on the root URL because wrangler defaulted to the `master` branch (preview), not `main` (production). Fixed by explicitly passing `--branch main`.

**Team Faith deploy command:**
```bash
cd "C:/Users/RAS/projects/team-dashboard" && npm run build && npx wrangler pages deploy dist --project-name team-faith --branch main --commit-dirty=true
```

**Live URL:** https://team-faith.pages.dev (accessible at RA's workplace; `.dev` TLD not blocked unlike `vercel.app`)
