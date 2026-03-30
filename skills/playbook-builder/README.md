# Playbook Builder

**Interactive single-page playbooks that turn strangers into clients.**

Two modes for two stages of the JICATE sales funnel:

| Mode | For Who | What It Creates |
|------|---------|----------------|
| **Industry** | Strangers ("I don't know JICATE") | Emotional 8-section playbook showing AI impact on their industry |
| **Company** | Warm leads ("I'm curious") | Technical 13-section X-ray of their specific business |

## What the Client Gets

### Industry Playbook (for strangers)
- 8 emotionally-driven sections: industry pain → AI solution → before/after → cost calculator
- 90% benefits, 10% technical credibility
- Interactive HTML deployed to Vercel
- Ends with a CTA: "Get your free Company X-ray"

### Company X-ray (for warm leads)
- 13 technical sections: company profile → gap analysis → competitor positioning → architecture recommendation
- Personalized using Firecrawl website scraping
- Shows THEIR specific data, not generic industry stats
- Ends with a CTA: "Book a walkthrough"

## How It Works

```
Industry Mode:
  "build a playbook for textile manufacturing"
       ↓
  Research industry + AI trends + pain points
       ↓
  Interactive HTML playbook → deployed to Vercel

Company Mode:
  "build a playbook for --company 'GLCS' --url 'glcs.in'"
       ↓
  Firecrawl website + research company + competitive analysis
       ↓
  Personalized X-ray → deployed to Vercel
```

## Commands

| Trigger | What it does |
|---------|-------------|
| `build a playbook for [industry]` | Industry mode — emotional playbook |
| `build a playbook for --company [name] --url [site]` | Company mode — technical X-ray |
| `--update` | Version management for existing playbooks |

## Features

- Interactive HTML with embedded analytics (Supabase tracking)
- White-label support (JICATE branding or custom)
- Vercel auto-deployment
- Mobile responsive

---

**Part of [JICATE Marketplace](https://github.com/Ommsharravana/jicate-marketplace)**
