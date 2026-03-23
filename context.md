# RA Context Sync
Last updated: 2026-03-24 4:30 AM PHT

## Who Is RA
- Ronald Adrian Sarinas, Manila, Philippines (PHT, UTC+8)
- Home: San Joaquin, Pasig
- Partner: Arabelle
- Baby: Baby Jah
- Cat: Buki
- Business: DuberyMNL (polarized sunglasses) + RAS AI SOLUTIONS (AI automation service brand)

## Available Files
Only fetch these when relevant to the conversation -- not all at once:
- schedule.md -- fixed weekly routines (RA + Arabelle)
- calendar.md -- specific dates, appointments, events
- family.md -- Baby Jah, Arabelle, home life
- health.md -- food, sleep, exercise, vices
- finances.md -- spending, bills, income, assets
- vehicles.md -- car, scooter, maintenance, gas
- home.md -- house stuff, bills, maintenance
- pets.md -- Buki the cat
- plans.md -- future plans, goals, travel
- lifestyle.md -- hobbies, interests, media, style
- skillset.md -- technical, creative, professional skills

Work context (code, project details):
- https://raw.githubusercontent.com/RASCLAW/DuberyMNL/main/CLAUDE.md

## Last 3 Days

### 2026-03-24 (Tuesday -- at work, shift 8pm-5am)
**Built the family dashboard + EA system overhaul + portfolio site.**
- Built ra-dashboard: family dashboard with RA + Arabelle profiles, live weather (Open-Meteo), PH holidays (Nager.Date), Baby Jah tracking
- Deployed to Vercel: ra-dashboard-lake.vercel.app
- Built system-loadout skill (session startup sequence)
- Built session-closeout skill (AFK = push everything)
- Built 7 /save slash commands
- Consolidated 15 scattered memory files into 3 clean behaviour files
- Added REFERENCES.md to all 19 skills for dependency tracking
- Improved session checkpoint script (90-min window, activity summary)
- Dashboard has: live clock (PHT), commute weather rec for Arabelle, Baby Jah card, RA sleep/food tracking, dark/light mode, event countdowns
- Built send_email.py for no-prompt emails to RA
- Exploring Tapo CCTV integration for dashboard (need camera IP + RTSP credentials at home)
- **Built RAS AI Solutions portfolio site** (`/home/ra/projects/ras-portfolio/`)
  - Main site: case study, skills, services, timeline
  - Interactive simulator: full content pipeline + CRM customer journey
  - Ad staging with Meta Ads API integration (shows actual API calls)
  - Autonomous error recovery scene (Meta rejects ad -> agent fixes + resubmits)
  - Family easter eggs: Arabelle, Bujah, Buki hidden in the demo

### 2026-03-23 (Monday -- shift starts tonight 8pm)
**MASSIVE DAY. Ads are LIVE.**
- 10 Meta Ads running (5 PERSON anchor + 5 PRODUCT anchor), P200/day
- Day 1 results: 77 landing page views, P116 spent, P1.51 avg cost per click
- Top performers: #22 (33 clicks), #6 (25 clicks), #20260320-002 (P0.73/click cheapest)
- Bought **duberymnl.com** domain (Namecheap), connected to Vercel
- Landing page deployed: full desktop + mobile layout, dark mode default
- Landing page features: variant galleries (swipeable), feedback carousel, custom dropdown with product thumbnails, order form with express delivery option
- Order pipeline working: form -> Google Apps Script -> Google Sheet
- FB auto-replies updated: correct pricing (P699/P1,200), COD, duberymnl.com links
- DTI Business Name Registration submitted: DUBERYMNL (Ref: BVZE438718986529, 1-2 days)
- Meta Business Verification submitted (2 business days)
- Email forwarding set up: ras@duberymnl.com -> sarinasmedia@gmail.com
- Google Drive fully reorganized: 100+ root files -> clean folder structure
- Phone photo sync working: FolderSync app, mobile-cam + mobile-photos folders in Drive
- Google Photos API integration attempted -- BLOCKED (Google removed photoslibrary.readonly scope April 2025)
- Got brake shoes replaced on the Vios (car maintenance)
- Planning road trip to Daet, Camarines Norte (Arabelle's hometown) end of month

### 2026-03-22 (Sunday -- rest day)
- Named the service brand: **RAS AI SOLUTIONS**
- Deep dive on GoHighLevel (GHL)
- Edited old skateboarding video in CapCut desktop
- Set up ClaudeMob + ra-sync repo
- Life tracking system designed (12 files)
- 12:56 AM snack, 2:40 AM chicken nuggets
- Baby Jah woke up ~2:30 AM

## ClaudeMob Notes
- RA wants unconscious expense tracking -- receipt screenshots, ClaudeMob logs
- PureGold receipts: snap 2-3 photos, ClaudeMob stitches
- GCash/Maya screenshots for digital payments
- Track recurring patterns, flag missing ones
- Sensitive data: amounts OK, last 4 digits only for accounts

## Pending / Next Up
- Monitor ad performance (day 2-3, check back tomorrow)
- Wait for DTI cert + Meta Business Verification (1-2 business days)
- After verification: publish Meta app, automate ad staging via API
- Delete Google Photos library (takeout backup done, 28GB on PC) -- free up Drive storage
- Upload2Drive folder set up on PC for easy Drive uploads
- FolderSync running: mobile-cam + mobile-photos syncing to Drive
- Top up kie.ai credits for remaining regens
- Deploy ras-portfolio to Vercel (currently local/ngrok only)
- Road trip to Daet end of March (car ready, brake shoes done)
- PC storage: cleared 4GB temp, 3.1GB recycle bin pending empty
