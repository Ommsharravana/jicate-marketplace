---
name: playbook-builder
description: "Generate interactive single-page HTML playbooks for JICATE Solutions sales funnel. Two modes: (1) Industry mode (V2) — 'build a playbook for [industry]' creates an 8-section emotional playbook that makes prospects FEEL the transformation (cost calculator, before/after timeline, X-ray CTA). No technical jargon. 90% benefits, 10% credibility. (2) Company mode — 'playbook-builder --company [name] --url [website] --contact [person]' creates a private 13-section technical X-ray for a specific company (gaps, competitors, architecture). The public playbook creates DESIRE; the private X-ray creates EVIDENCE. Also triggers on 'authority playbook', 'industry playbook', 'create playbook for', or 'update playbook'. Supports JICATE Solutions branding (default) and white-label for paid clients. Do NOT use for blog posts or articles (use content-pipeline), single-page websites (use vibe-coder), or marketing campaigns (use creative-strategist)."
---

# Playbook Builder

Two-stage sales funnel for JICATE Solutions. Each stage has a different job:

- **Industry playbook (V2, public)** — 8 emotional sections. Attracts STRANGERS. Makes them feel seen, quantifies their pain (calculator), shows the transformation, and funnels them to request a free X-ray. Think: mattress ad that shows someone waking up refreshed.
- **Company playbook (private)** — 13 technical sections. Closes WARM LEADS after a meeting/event. Shows the prospect their own data — their website gaps, their competitors' advantages, their GSTIN. Think: doctor showing you YOUR x-ray, not a textbook diagram.

The stranger sees the emotional playbook → fills the X-ray form → JICATE builds the company playbook → prospect calls.

Standard HTML/CSS, Vercel deployment, and Firecrawl usage are assumed knowledge.

## CRITICAL: Mode Routing (Read This First)

> **Industry mode = V2 (8 sections). DEFAULT.**
> **Company mode = V1 (13 sections). ALWAYS.**
>
> If the user asks for an industry playbook, use `references/phase-2-content-v2.md` (8 emotional sections) by default.
> The 13-section technical structure lives in `references/phase-2-content.md` and is used for:
> 1. Company mode (`--company` flag) — always
> 2. Industry mode with `--v1` or `--technical` flag — when user explicitly asks for the technical/authority version
> 3. When user says "use 13 sections", "technical playbook", "authority playbook with architecture", or similar explicit requests

## 5-Phase Workflow

1. **Research** — 3 parallel agents gather 30+ substantiated data points
2. **Content** — Generate playbook sections using research output
   - **Industry mode (V2):** 8 emotional sections — read `references/phase-2-content-v2.md`. Do NOT read `references/phase-2-content.md` — that file is for company mode only.
   - **Company mode:** 13 technical sections — read `references/phase-2-content.md` + `references/company-mode.md`
3. **HTML** — Build interactive single-page HTML matching the notebook/paper template design system
4. **Analytics** — Inject tracking script (or no-op stub if Supabase not configured)
5. **Deploy** — Human review gate, then Vercel deployment + registry update

## Two-Stage Sales Funnel

| Stage | Mode | Sections | Purpose |
|-------|------|----------|---------|
| **Public playbook** | Industry mode (V2) | 8 emotional | Create DESIRE — "I want this life" |
| **Private X-ray** | Company mode | 13 technical | Create EVIDENCE — "Here's YOUR gaps" |

The prospect sees the emotional playbook, fills the X-ray form, and JICATE delivers the technical analysis privately. Technical depth moved from public to private.

## Mode Detection

If the user provides `--company`, `--url`, or mentions a specific company name with a website, enter **Company Mode**. Otherwise, use **Industry Mode** (default).

## Quick Interview — Industry Mode (Ask Before Building)

1. **Industry/domain:** "What industry is this playbook for?" (e.g., pharmacy chains, dental clinics)
2. **Target audience:** "Who reads this? What's their title?" (e.g., chain owners, clinic directors)
3. **Geography:** "India-focused, or global?" (affects stats, compliance, currency)
4. **Insider knowledge:** "Any specific pain points, stats, or insights from this industry?" (optional)
5. **Branding:** "JICATE Solutions branding, or white-label for a client?" — If white-label: client name, primary color hex, CTA email/URL

## Quick Interview — Company Mode (3 Questions Only)

1. **Confirm details:** "Building a personalized playbook for [Company]. URL: [url]. Contact: [contact]. Correct?"
2. **Conference context:** "What event did you meet them at? Any specific pain they mentioned?"
3. **Branding:** "JICATE Solutions branding, or white-label?"

## Pre-Flight Checks

1. **Concurrency guard:** Read `/Users/omm/PROJECTS/jicate-playbooks/registry.json` — abort if same slug has `"status": "in-progress"`
2. **Industry scoping (industry mode only):** If industry is a top-level category (healthcare, manufacturing, education, retail, agriculture, finance, logistics, construction, hospitality, media), ask user to narrow to a segment
3. **URL reachability (company mode only):** Firecrawl scrape the homepage. If it fails, abort: "Cannot reach [url]. Verify the URL or try again later."
4. **Firecrawl credits:** Run `firecrawl --status` to verify sufficient credits before committing to research
5. **Bootstrap:** If `/Users/omm/PROJECTS/jicate-playbooks/` does not exist, create it with empty `registry.json` and `registry-config.json`

## Knowledge Graph

Start with `references/phase-1-research.md` — it defines the 3 parallel research agents, fallback chains, and the research output schema that feeds all subsequent phases.

**For industry mode (V2) — THIS IS THE DEFAULT:** Read `references/phase-2-content-v2.md` for the 8-section emotional structure, 90/10 rule, banned technical jargon, cost calculator spec, before/after timeline, and X-ray CTA form. **You MUST use this file for industry playbooks. Do NOT read `references/phase-2-content.md` — that is company mode only and will produce the wrong output.**

**For company mode ONLY (requires `--company` flag):** Read `references/phase-2-content.md` for the 13-section technical structure (used in private X-ray deliverables only).

When building the HTML file, read `references/phase-3-html.md` for the template structure, interactive element specs, and white-label sanitization rules. Also read `references/design-system.md` for exact colors, fonts, and component styles.

When injecting analytics, read `references/phase-4-analytics.md` for the tracking script, Supabase schema, Edge Function security, and privacy/consent requirements.

When deploying and packaging, read `references/phase-5-deploy.md` for the human review gate, Vercel deployment, registry management, and paid client deliverables.

Before marking any phase complete, read `references/quality-gates.md` for the 9 quality gates, banned phrases, retry limits, and failure handling.

For SEO metadata and social sharing tags, read `references/seo-metadata.md` before finalizing the HTML.

For `--update` invocations, read `references/phase-5-deploy.md` for version management — old version archived, new version deployed to same URL.

For `--company` invocations, read `references/company-mode.md` for the complete company mode specification: restructured research agents, content personalization rules, auto-detection of currency/geography/regulatory bodies, privacy safeguards, and Gate 10 (Privacy Compliance). Also read `/Users/omm/PROJECTS/jicate-playbooks/reference/localization.json` for the country/currency/regulatory mapping data.

## Troubleshooting

### Research agents return thin results

```
# WRONG — proceed with sparse research
"Only found 8 data points, generating content anyway..."

# CORRECT — exhaust fallback chain before proceeding
"Research fell short (8/30 points). Trying fallback: Firecrawl → WebSearch → WebFetch.
Still under threshold → asking user for insider knowledge to supplement."
```

The minimum is 30 substantiated data points. If all fallback chains are exhausted and you're still under, ask the user — they often have industry stats that aren't online.

### Currency symbol wrong (£ instead of ₹)

```
# WRONG — using pound symbol for Indian currency
"₹25,000" rendered as "&pound;25,000" in HTML

# CORRECT — use ₹ directly or HTML entity &#8377;
"₹25,000" or "&#8377;25,000" — NEVER use &pound; for Indian Rupee
```

When generating HTML for India geography, always use the ₹ symbol (Unicode U+20B9, HTML entity `&#8377;`). The `&pound;` entity (£) is British Pounds — a completely different currency. This is a critical factual error that destroys credibility with Indian prospects.

### Playbook reads like a generic blog post

```
# WRONG — vague advice anyone could write
"Businesses should consider adopting AI to improve efficiency."

# CORRECT — specific, data-backed, problem-first
"73% of Indian pharmacy chains still reconcile inventory manually,
costing ₹2.4L/month in shrinkage. Here's the 4-stage automation path."
```

If content feels generic, the research phase likely failed. Go back and verify data points have real numbers, sources, and India/geography-specific context.

### Firecrawl credits exhausted mid-research

Run `firecrawl --status` before starting. If credits are low, the pre-flight check should catch this. If it happens mid-run, switch to WebSearch + WebFetch fallback (no Firecrawl needed) and note the degraded research quality in the registry.

## Example Invocation — Industry Mode (V2)

User says: *"build a playbook for dental clinic chains in India"*

1. Quick interview → industry: dental clinics, audience: clinic directors, geography: India, branding: JICATE Solutions
2. Pre-flight → check registry, verify Firecrawl credits, scope is narrow enough (not "healthcare")
3. Phase 1 → 3 research agents run 10+ minutes each, produce 35+ data points
4. **Phase 2 → V2 emotional content (8 sections): wake-up call stories, cost calculator, before/after timeline, intelligence preview, JICATE process, case study narrative, X-ray CTA form, soft close. Uses ₹ currency, India-specific pain points, second person present tense ("You open your phone..."). Read `references/phase-2-content-v2.md` — NOT `references/phase-2-content.md`.**
5. Phase 3 → Interactive HTML with **cost calculator** (slider inputs → ₹ result), **before/after timeline** (two-column), **X-ray request form** (with validation + honeypot), sticky 8-section nav
6. Phase 4 → Analytics stub (or live tracking if Supabase configured)
7. Phase 5 → Human review → `vercel --prod` → registry updated → URL returned

Output: `https://jicate-playbooks.vercel.app/dental-clinic-chains-india/`

## Example Invocation — Company Mode

User says: *"/playbook-builder --company "ABC Dental" --url "abcdental.com" --contact "Dr. Sharma, Director""*

1. Quick interview → confirm details, ask about conference context, branding: JICATE Solutions
2. Pre-flight → check registry, verify Firecrawl credits, test URL reachability
3. Phase 1 → 3 agents: Company Intel (scrape abcdental.com + reviews), Competitor Landscape (find 3-5 competitors in same city), Industry Context (broader dental market stats)
4. Auto-detect → India (from Chennai address on contact page; .com TLD is inconclusive per localization.json), ₹ currency, DCI regulatory body
5. Phase 2 → 13-section content personalized to ABC Dental: their gaps, their competitors, their geography
6. Phase 3 → Interactive HTML with `noindex` meta tag, company name throughout, contact in CTA
7. Phase 4 → Analytics stub
8. Phase 5 → Human review ("This references ABC Dental 24 times...") → `vercel --prod` → registry updated

Output: `https://jicate-abc-dental-chennai-playbook.vercel.app/`
