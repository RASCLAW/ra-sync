---
name: feedback_vercel_deploy_cloudflare
description: Vercel CLI deploy broken when Cloudflare proxy is active -- use git push instead
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 04d5c06f-7ed0-4aa9-a32f-e97449bee77e
---

Always deploy duberymnl.com via `git push origin main`, NOT `vercel --prod`.

**Why:** When Cloudflare proxy (orange cloud) is active on duberymnl.com and www.duberymnl.com, `vercel --prod` CLI produces deployments with UNKNOWN status and [0ms] build time. They upload fine but never get aliased to the production domain. The site stays on the old version indefinitely. Discovered 2026-05-15 after multiple CLI deploys all showed UNKNOWN.

**How to apply:** Always commit changed landing page files and `git push` to trigger Vercel's GitHub integration. That path bypasses the CF proxy issue and properly promotes the deploy to production.
