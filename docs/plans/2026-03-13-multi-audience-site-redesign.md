# Multi-Audience Site Redesign

**Date:** 2026-03-13
**Status:** Approved

## Summary

Restructure safe4seniors.ai from an investor-heavy single-audience page to a family-first, multi-audience marketing site serving three groups: families/end-users, B2B partners (AAAs, health systems, tribes), and investors.

## Key Decisions

1. **Family-first flow** (Option A) — Hero speaks to families, investor/partner detail lives below the fold
2. **Consolidate aggressively** (Option A) — Replace PMPM tables, Medicare billing codes, and financial projections with lean "For Investors" and "For Partners" sections
3. **Dual Mission as its own section** (Option B) — Dedicated section for healthcare AI + Nimipuutímt language preservation
4. **Language preservation threaded through three layers** (Option C) — Hero mention, Technology section, and full Dual Mission section

## New Page Architecture

| # | Section | ID | Audience | Status |
|---|---------|-----|----------|--------|
| 1 | Nav | `main-header` | All | Updated |
| 2 | Hero | — | Families (primary) | Rewritten |
| 3 | How It Works | `how-it-works` | Families | New |
| 4 | Demos | `demos` | All | Unchanged |
| 5 | Technology | `technology` | All (credibility) | New |
| 6 | For Partners | `partners` | AAAs, health systems, tribes | New (replaces old) |
| 7 | For Investors | `investors` | Investors | New (replaces Business Model + old) |
| 8 | Dual Mission | `mission` | Grant funders, impact investors | New |
| 9 | Team | `team` | All | Updated |
| 10 | Contact/CTA | `contact` | All | Updated |
| 11 | Footer | — | All | Minor update |

## Sections Removed

- Dual-Pilot Advantage Banner (gold strip) — absorbed into For Investors
- Solution section (Problem / What We Do / Why Now) — replaced by How It Works
- Platform Ready section — absorbed into Technology
- Business Model section (PMPM tables, Medicare codes, projections, unit economics) — replaced by For Investors
- Dual-Pilot Details section (demographic cards, validation axes table) — key facts into For Investors and For Partners
- Old Partners section (3 audience cards) — replaced by For Partners + For Investors

## Section Details

### Hero

- Headline: "Your loved one. Checked on. Every day."
- Subtitle: "AI-powered daily wellness calls for seniors aging in place — no app, no internet required."
- Tertiary: "Also preserving the Nimipuutimt language with the Nez Perce Tribe." (subtle, smaller text)
- CTA left: "See How It Works" -> #how-it-works (gold button)
- CTA right: "Partner With Us" -> #partners (outline/ghost button)
- Stats row: Keep 4 stat cards (13,200+ seniors, 2 Pilots, 2 States, $30-60 PMPM)

### How It Works

Three cards, extremely simple:
1. "We Call" — phone icon — "Sunny calls your loved one every day at their preferred time. No app to install, no internet needed — just their regular phone."
2. "We Listen" — conversation icon — "A warm, natural conversation — medication reminders, mood check-ins, home safety questions. AI that sounds like a friend, not a robot."
3. "We Alert" — bell icon — "If something's off — a missed call, a fall, a worrying pattern — family and caregivers are notified immediately."

### Demos

Unchanged from previous session (three video cards).

### Technology

- Headline: "Enterprise-Grade AI. Any Phone."
- Subtitle: "Same platform powers daily wellness calls in English and Nimipuutimt language preservation with the Nez Perce Tribe."
- Three tech items inline: Amazon Connect (telephony), NVIDIA Riva (speech AI), Claude via Bedrock (conversational AI)
- Bottom bar: "No app required . Works on any phone . HIPAA compliance path (BAA in progress)" + NVIDIA Inception badge

### For Partners

- Headline: "Integrate Sunny Into Your Care Network"
- Subtitle: "Free 3-month pilot. No integration cost. Measurable outcomes."
- 2x2 grid of cards:
  - AAA Integration: Region II and Region IV, 622 AAAs nationwide
  - Tribal Health: Nimiipuu Health / Nez Perce, dual-purpose health + language
  - Health Systems: Tri-State Memorial, St. Luke's Wood River, St. Joseph Regional
  - Device Integrations: Dexcom G7 CGM, CPAP/ResMed monitoring

### For Investors

- Headline: "The Aging-in-Place Market Is Massive and Underserved"
- Left column — key metrics: $1M seed (raising now), 55M+ Americans 65+, 622 AAAs, Series A post-pilot
- Right column — strategy prose: dual-pilot validation, NVIDIA Inception, tech stack, timeline
- CTA: "Request Investment Deck" -> mailto

### Dual Mission

- Headline: "Two Missions. One Platform."
- Two equal cards:
  - Primary: "Healthcare AI for Aging in Place"
  - Secondary: "Nimipuutimt Language Preservation" (fewer than 100 fluent speakers)
- Bottom quote: "The Nez Perce Tribe is our proof-of-concept partner..."

### Team (Updated)

- Jay Myers: Co-Founder / CEO — cross-disciplinary: AI/ML, construction PM, GIS
- Dr. Richard Stanley: Co-Founder / CHO — PhD Oxford, PATH, IntraHealth, UNICEF
- Updated "Why This Team Works" callout

### Contact / CTA

- Headline: "Let's Talk"
- Two cards: "Schedule a Demo" (partners/investors) + "Learn More for Your Family" (consumers)

### Footer

- Updated nav links to match new section IDs

## Technical Constraints

- Tailwind CDN + vanilla JS only (no npm, no frameworks)
- Existing brand palette: brand-blue #1e3a5f, brand-blue-light #2c5282, brand-green #38a169, brand-gold #d69e2e
- Keep existing animation classes (fade-up, card-hover, btn-primary)
- Keep all existing SVG assets
- Sync public/ and marketing/ directories
