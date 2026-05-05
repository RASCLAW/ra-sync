---
name: Dashboard Moderator Skill
description: /dashboard-moderator skill lives in ra-dashboard repo, syncs family data and deploys to Vercel.
type: reference
related: [user_arabelle.md, user_zach.md]
---

The `/dashboard-moderator` skill is at `ra-dashboard/.claude/skills/dashboard-moderator/SKILL.md`.

It syncs life-log, finances, baby tracking, bills, and other family data into dashboard-db.json, then deploys to Vercel.

Workflow: assess staleness -> ask RA for updates -> present change plan -> update DB -> deploy -> log to moderator-log.md.

Next step: wire as a scheduled remote agent for automatic syncing.
