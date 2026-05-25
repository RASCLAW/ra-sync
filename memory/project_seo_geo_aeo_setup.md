---
name: seo-geo-aeo-setup
description: "Phased plan to wire SEO + GEO (Generative Engine Optimization) + AEO (Answer Engine Optimization) into duberymnl.com v3 site. HIGH PRIORITY (priority #7), not top. Scoped 2026-05-20."
metadata: 
  node_type: memory
  related: 
    - project_dubery_v3_landing
    - project_messenger_strategy
  type: project
  originSessionId: c829ae80-8117-4adc-93e0-481ebc1c3c8b
---

# SEO / GEO / AEO setup for duberymnl.com

Scoped during 2026-05-20 discussion. **Priority #7 in current-priorities.md -- HIGH but not top** (chatbot 1-week production data is still TOP). Goal: free organic traffic to complement Meta ads, brand defense in LLM answers, AEO snippet capture in Google AI Overview.

## Definitions
- **SEO** -- Google/Bing organic ranking (the classic).
- **GEO** -- Generative Engine Optimization. Get cited by ChatGPT, Gemini, Perplexity, Claude when users ask shopping questions.
- **AEO** -- Answer Engine Optimization. Win Google's AI Overview box, featured snippets, voice assistant answers.

## Why bother (without Google Ads)
1. Compounds -- write once, pulls traffic for years; ads stop the day spend stops.
2. Brand defense -- when someone Googles "Dubery review" or asks ChatGPT, you control the narrative.
3. Captures research-mode buyers (top of funnel) that FB ads miss.
4. Doubles as RAS Creative portfolio proof ("ranked my own brand on Google + ChatGPT without paid spend") -- the exact pitch a solar installer needs to hear.

## Current state of v3 site
| Item | Status |
|---|---|
| HTTPS, mobile responsive, fast (Vercel) | done |
| Clean URLs (e.g. `/bandits-tortoise`) | done |
| Meta tags per page | likely basic or missing -- needs audit |
| Schema.org JSON-LD | none |
| sitemap.xml | none |
| robots.txt | unknown |
| Google Search Console submission | not done |
| FAQ page | not built |
| llms.txt | not built |
| Off-site mentions (Reddit/Quora) | none |

## Phased plan

### Phase 1 -- Technical foundation (~3-4 hrs, one session)
Knocks out SEO + GEO base layer in one swing.
1. Audit + write unique `<title>` (~60 chars) + `<meta description>` (~155 chars) per page (home, /order, every PDP).
2. Open Graph + Twitter Card tags so FB/IG/X share previews are clean.
3. Schema.org JSON-LD blocks in `<head>`:
   - `Product` on every PDP (name, price 499, currency PHP, availability, brand Dubery, image, description)
   - `Organization` on homepage
   - `BreadcrumbList` on PDPs
4. Auto-generated `sitemap.xml` (one entry per PDP + landing + order).
5. `robots.txt` -- allow all, point to sitemap.
6. `llms.txt` at root -- plain-text brand summary for AI crawlers (emerging standard).
7. Submit site + sitemap to Google Search Console (free). Bing Webmaster Tools optional.

### Phase 2 -- FAQ page (~2-3 hrs)
Single highest-leverage content asset. Hits SEO + GEO + AEO simultaneously.
1. Pull 15-20 real questions from Messenger logs (`conversation_store.json` is goldmine).
2. Build /faq page with Q-format H2/H3 headings ("How much do Dubery sunglasses cost?", "Are Dubery polarized?", "Do you ship to provinces?").
3. 40-60 word direct answer immediately under each Q (Google snippet sweet spot).
4. `FAQPage` schema JSON-LD on the page -- qualifies for "People Also Ask" boxes.
5. Tables/lists where applicable (e.g. comparison table of Bandits / Outback / Rasta).

### Phase 3 -- Off-site (ongoing, slow)
1. One organic Reddit thread (r/Philippines or r/phbuyandsell) -- not spam, an actual helpful comparison post.
2. One blog post on duberymnl.com targeting buyer-intent keyword (e.g. "best affordable polarized sunglasses Philippines").
3. Re-evaluate after Phase 1+2 have 60 days of Search Console data.

## Tradeoff to remember
SEO/GEO/AEO is slow -- 3-6 months minimum to see needle move. Run as parallel low-stakes track, never a swap for ads. Do not touch during DuberyMNL chatbot 1-week production data window (priority #1 gate).

## How to pick this up
When ready: `/plan` Phase 1 task list against the v3 site files. Pair with `/execute`. Phase 1 is a clean fit for one focused session and the site is the right base.
