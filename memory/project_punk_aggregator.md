---
name: Manila Punk Scene Aggregator
description: Automated Facebook page + website covering Manila's local punk/underground music scene -- gig listings, content curation, chatbot
type: project
related: [reference_apify_setup.md]
---

Automated Facebook page that covers Manila's lokal punk scene. Scrapes YouTube, Facebook, and web for gig info, band news, and scene content. Auto-posts, responds to comments/messages, publishes website articles.

**Why:** Zero competition -- Manila punk scene is 100% manual (FB events + word of mouth). No aggregator exists. Closest global model is Gigigator (scrapes FB band/venue pages, auto-aggregates events). RA genuinely likes this project.

**Stack:** Same DuberyMNL pipeline (content gen, auto-posting, chatbot) + scraper layer. YouTube Data API (free), Facebook Graph API for public pages, web scraping for gig listings. Claude for content summarization. Static Vercel site for articles.

**Status:** BACKLOG -- start after DuberyMNL Messenger funnel ships. Start small: follow pages, aggregate gig listings, auto-post. Expand from punk to broader Manila underground (indie, hardcore, metal, ska).

**How to apply:** When DuberyMNL pipeline is proven and reusable, this becomes the second portfolio piece proving the system works across niches. Don't start building until priority #2 (chatbot + Messenger funnel) is done.
