---
description: "Company mode — personalized playbooks for specific companies met at conferences. Restructured agents, content personalization, localization, privacy safeguards."
---

# Company Mode: Personalized Playbooks for Specific Companies

Generate a playbook tailored to a SPECIFIC company (prospect met at a conference), not a generic industry playbook. The system scrapes their website, identifies gaps, compares competitors, and produces a custom 13-section analysis within 30-60 minutes.

## Invocation

```
/playbook-builder --company "ABC Dental" --url "abcdental.com" --contact "Dr. Sharma, Director"
```

**Required arguments:**
- `--company` — Company name (triggers company mode)
- `--url` — Company website URL

**Optional arguments:**
- `--contact` — Contact person's name and role (e.g., "Dr. Sharma, Director")

If `--company` flag is present, the skill enters company mode. If absent, industry mode (existing behavior) is used.

**If `--company` is provided without `--url`:** Ask "What is [Company Name]'s website URL?" before proceeding. URL is required for company mode.

**URL normalization:** If `--url` does not begin with `http://` or `https://`, prepend `https://`.

**Slug sanitization:** Lowercase the company name, replace spaces with hyphens, strip all non-alphanumeric characters except hyphens. Example: "Dr. Smith & Partners" → "dr-smith-partners". Verify the final Vercel project name (`jicate-[slug]-playbook`) is under 60 characters.

## Quick Interview (3 Questions Only)

Company mode needs less interview because the website provides most context.

| # | Question | Why |
|---|----------|-----|
| 1 | "Building a personalized playbook for [Company]. URL: [url]. Contact: [contact]. Correct?" | Prevent errors from misheard business card details |
| 2 | "What event did you meet them at? Any specific pain they mentioned in conversation?" | Conference conversations contain insights no scrape will find |
| 3 | "JICATE Solutions branding, or white-label for a client?" | Same as industry mode |

## Pre-Flight Checks (Company Mode Additions)

In addition to the standard pre-flight checks (registry concurrency, Firecrawl credits):

1. **URL reachability:** Firecrawl scrape the homepage. If it fails (timeout, 403, 404), abort: "Cannot reach [url]. Verify the URL or try again later."
2. **Duplicate check:** Check registry for any entry with same `companyUrl`. If found with `"status": "deployed"`, ask: "A playbook for [Company] already exists ([url]). Create a new version or abort?"
3. **Registry entry:** Add immediately with `"mode": "company"`, `"status": "in-progress"`

## Phase 1: Research — 3 Restructured Agents

### Agent 1: Company Intel Agent (replaces Pain Hunter)

**Purpose:** Deep scrape of the TARGET company to understand who they are and where they fall short.

**Research steps:**
1. `firecrawl map [url]` — discover all pages on the site
2. `firecrawl scrape` key pages: homepage, about, services, team, contact, testimonials, pricing (if public)
3. WebSearch: "[Company Name] reviews", "[Company Name] complaints", "[Company Name] news [year]"
4. WebSearch: "[Company Name] LinkedIn" — company size, employee count (public data only)
5. WebSearch: "[Company Name] Google Maps" — rating, review count, common themes

**Auto-detection from scrape results:**

| Signal | How to Detect | Output Field |
|--------|--------------|--------------|
| Country | TLD (.in, .co.uk), address on contact page, phone prefix | `geography.country` |
| Currency | Country → lookup `localization.json` | `geography.currencySymbol` |
| Industry segment | Services page content, meta description | `industrySegment` |
| Company size | Location count, team page count, "About" copy | `sizeEstimate` |
| Tech maturity | Online booking? Patient portal? Mobile app? Chat widget? SSL? | `techVisible` object |
| Visible gaps | What competitors have that this company lacks | `visibleGaps` array |
| Regulatory environment | Country + industry → `localization.json` lookup | `regulatoryBodies` array |

**Output schema:**
```json
{
  "agent": "company-intel",
  "company": "ABC Dental",
  "companyUrl": "abcdental.com",
  "dataPoints": [
    {
      "id": "CI-001",
      "claim": "ABC Dental has 3 locations in Chennai but no online booking on any",
      "source": "abcdental.com",
      "sourceUrl": "https://abcdental.com/contact",
      "type": "pattern",
      "targetSection": "bleeding-areas",
      "confidence": "high"
    }
  ],
  "companyProfile": {
    "name": "ABC Dental",
    "tagline": "Extracted from homepage",
    "services": ["Orthodontics", "Implants", "Cosmetic"],
    "locations": [{"city": "Chennai", "address": "..."}],
    "teamSize": "15-25 estimated",
    "techVisible": {
      "hasOnlineBooking": false,
      "hasPatientPortal": false,
      "hasMobileApp": false,
      "hasChatWidget": true,
      "hasSSL": true,
      "mobileResponsive": true,
      "socialMedia": ["Instagram", "Facebook"]
    },
    "visibleGaps": [
      "No online booking — patients must call",
      "No patient portal",
      "No digital intake forms"
    ],
    "geography": {
      "country": "India",
      "countryCode": "IN",
      "state": "Tamil Nadu",
      "city": "Chennai",
      "currencySymbol": "₹",
      "currencyCode": "INR",
      "numberFormat": "indian"
    },
    "regulatoryBodies": ["DCI", "State Dental Council of Tamil Nadu"]
  },
  "sourcesAccessed": ["https://abcdental.com", "https://abcdental.com/about", "..."],
  "queryCount": 15,
  "timestamp": "2026-03-18T10:30:00Z"
}
```

**Minimum gate:** 15+ data points, including 3+ visible gap findings.

**XSS prevention (MANDATORY):** All company-provided strings (company name, contact name, services, tagline) MUST be HTML-encoded before injection into playbook HTML. Apply the same sanitization pipeline defined in `phase-3-html.md` White-Label Input Sanitization section: `&` → `&amp;`, `<` → `&lt;`, `>` → `&gt;`, `"` → `&quot;`, `'` → `&#x27;`.

### Agent 2: Competitor Landscape Agent (replaces Stat Collector)

**Purpose:** Find 3-5 direct competitors in the same geography and size range, compare capabilities.

**Research steps:**
1. WebSearch: "[industry] [city]", "best [industry] in [city]", "[industry] near [company address]"
2. Firecrawl scrape top competitor websites: services, features, online capabilities
3. WebSearch: competitor Google Maps ratings and review counts
4. Compare: which competitors have capabilities the target company lacks

**Output schema:**
```json
{
  "agent": "competitor-landscape",
  "competitors": [
    {
      "name": "Competitor Name",
      "url": "competitor.com",
      "services": ["..."],
      "techAdvantages": ["Online booking", "Patient portal"],
      "googleRating": 4.5,
      "reviewCount": 320,
      "estimatedSize": "3 locations",
      "keyDifferentiator": "First to offer same-day crowns"
    }
  ],
  "competitiveGaps": [
    {
      "gap": "Online booking",
      "targetHasIt": false,
      "competitorsWithIt": ["Competitor A", "Competitor B"],
      "businessImpact": "Estimated 15-20% patient loss"
    }
  ],
  "dataPoints": [{"id": "CL-001", "...": "..."}]
}
```

**Minimum gate:** 8+ data points, 3+ competitor profiles.

**Privacy (MANDATORY):** The same public-sources-only rules that apply to the target company also apply to ALL competitor research. No competitor login-protected content, internal documents, or employee personal data beyond publicly listed name/title. Proactively check competitors' `/robots.txt` before scraping — skip blocked paths.

**Data point ID prefix:** CL- (e.g., CL-001, CL-002)

### Agent 3: Industry Context Agent (replaces Trend Scanner)

**Purpose:** Broader industry stats narrowed to THIS company's specific segment and geography.

Same approach as existing Trend Scanner, but search queries narrowed:
- Include the company's geography in every query
- Include the company's specific sub-segment
- Use the detected currency for all cost/revenue stats
- Reference the detected regulatory bodies

**Minimum gate:** 8+ data points. Each data point has a `geographyRelevance` field: "local", "national", or "global".

**Data point ID prefixes (company mode):** CI- (Company Intel), CL- (Competitor Landscape), IC- (Industry Context)

## Phase 2: Content Adaptation Rules

The 13-section structure is identical to industry mode. Content becomes company-specific:

### Section 1: Hero
- Title: "The AI-Native Playbook for **[Company Name]**"
- Subtitle: "Prepared for [Contact Name], [Contact Role]"
- If no contact provided: "Prepared for the [Company Name] leadership team"
- 4 stat boxes: Use company-specific stats from research (e.g., "[N] locations", "[N] competitors with online booking", their Google rating, market growth rate)
- Sticky note: Same "how to use" format

### Section 2: The Problem
- Frame around THEIR specific situation: "A [size] [industry] practice in [city] with [N] locations faces..."
- Reference their visible gaps directly
- Use detected currency for all cost estimates
- Reference detected regulatory bodies

### Section 3: 5 Bleeding Areas
- Derive from gaps found in THEIR website + competitor comparison
- Each area says "[N] of your [N] competitors already have [capability]"
- Cost estimates in detected currency
- At least 1 area per visible gap from Company Intel Agent

### Section 4: The AI Shift
- "Before" column uses THEIR current state (scraped from website)
- "After" column shows what's possible for THEIR specific setup

### Section 5: What AI-Native Means
- Examples drawn from their specific sub-segment
- "For a [size] [sub-segment] practice like [Company Name]..."

### Section 6: Architecture
- Layer 3 (Sector Modules) customized to their SPECIFIC services
- Layer 4 (Infrastructure) customized to their SPECIFIC geography's compliance and payment systems (from `localization.json`)

### Section 7: 10 Must-Haves
- Criteria highlight gaps in THEIR current setup
- Each must-have has a subtle "Currently: [their status]" indicator based on website scrape
- Example: "3. Online Patient Booking — Currently: Not detected on your website. 4 of 5 competitors offer this."

### Section 8: Maturity Model
- Pre-scored based on public information from Company Intel Agent
- Display: "Based on your public web presence, we estimate [Company Name] is at **Level [X]**"
- Checkboxes still interactive for self-correction
- Pre-check boxes that are clearly true from the scrape

### Section 9: 30-60-90 Plan
- Actions specific to THEIR gaps and geography
- "Month 1: Deploy online booking (your competitor [X] gained [Y]% more patients after doing this)"
- Use detected currency and regulatory bodies
- Reference their specific payment systems from `localization.json`

### Section 10: Case Study
- Use the bridge from `reference/jkkn-case-study.md` adapted to their specific industry and scale
- "Like your [size] practice, this [similar-size] institution in Southern India..."

### Section 11: AI Prompts
- Prompts customized with THEIR company name, services, and geography
- "Analyze [Company Name]'s patient scheduling patterns for the [city] location"

### Section 12: Readiness Checklist
- Questions reference THEIR specific situation
- Pre-check items that are clearly true from website scrape (e.g., if they have SSL, pre-check the security question)

### Section 13: CTA
- "Dear [Contact Name], let's discuss what this means for [Company Name]."
- "No sales pitch — just a conversation about the [N] gaps we identified."
- If no contact: "Let's discuss what this means for [Company Name]."

## Content Quality Rules (Company Mode Additions)

In addition to all industry-mode quality rules:

| Rule | Threshold |
|------|-----------|
| **Personalization density** | At least 20 references to company name, contact name, geography, or specific services |
| **Competitor references** | At least 3 competitor comparisons |
| **Gap specificity** | Every bleeding area references a SPECIFIC finding from Company Intel Agent |
| **Pre-scoring disclosure** | Section 8 includes: "This pre-assessment is based on publicly available information. Use the checkboxes to correct our assumptions." |
| **Currency consistency** | All monetary values use the auto-detected currency from `localization.json` |
| **Regulatory accuracy** | All compliance references use the detected country's regulatory bodies |

## Currency and Number Formatting

When generating content, use the detected `numberFormat` from `localization.json`:

| Format | Example (1.25 million) | Where Used |
|--------|----------------------|-----------|
| `indian` | ₹12.5 lakhs or ₹1.25 crore | India |
| `western` | $1.25 million or $1,250,000 | US, UK, AU, CA, SG, UAE |

Rules:
- For values under 1 lakh (Indian) or 100K (Western), use exact numbers: ₹75,000 or $75,000
- For larger values, use denominations: ₹2.5 crore, $2.5 million
- Always use the detected currency symbol, never mixed currencies
- If a stat is from a different geography, convert and note: "approximately ₹X (converted from $Y)"

## Privacy Safeguards (MANDATORY)

### Data Sources: Strictly Public

| Allowed | NOT Allowed |
|---------|-------------|
| Company website (public pages) | Login-protected pages |
| Google search results | Private social media |
| Google Maps reviews | Internal documents |
| Public LinkedIn company pages | Employee personal profiles (beyond name/title) |
| News articles about the company | Financial filings (unless public/listed company) |
| Industry association directories | Client/patient databases |
| Public social media posts by the company | Private messages or groups |

### Mandatory Disclaimer

Every company-mode playbook includes a disclaimer (styled as a sticky note) in the hero section:

> "This analysis is based entirely on publicly available information about [Company Name], including your website, public reviews, and industry data. No private, internal, or confidential information was accessed or used. If any information is inaccurate, we welcome corrections."

### robots.txt Compliance

If Firecrawl reports that specific paths are blocked by `robots.txt`, those paths are skipped. Log as `"robotsBlocked": true` in research output. Do NOT attempt workarounds.

### Research Data Handling

- Research JSON saved to `[company-slug]/research.json` with `"dataClassification": "public-sources-only"`
- NOT committed to git (already in `.gitignore`)
- Company Intel Agent output includes `"sourcesAccessed"` array listing every URL scraped

## SEO & Deployment Differences

Company-mode playbooks are PRIVATE sales tools, not public authority content.

### Meta Tags
```html
<meta name="robots" content="noindex, nofollow">
```
- No JSON-LD structured data (not for search indexing)
- No canonical URL
- og:title and og:description still set (for email/WhatsApp link previews)

### Vercel Deployment
- Same flow: `vercel --prod --yes --name jicate-[company-slug]-playbook`
- No custom domain setup (one-off URLs)
- Registry tracks `"retention": "sales-cycle"` (default) or `"retention": "permanent"` (if requested)

## Registry Schema (Company Mode)

```json
{
  "id": "abc-dental-chennai",
  "mode": "company",
  "industry": "Dental Clinics",
  "companyName": "ABC Dental",
  "companyUrl": "abcdental.com",
  "contactName": "Dr. Sharma",
  "contactRole": "Director",
  "audience": "Dr. Sharma, Director",
  "geography": "India",
  "geoDetail": {
    "country": "India",
    "countryCode": "IN",
    "state": "Tamil Nadu",
    "city": "Chennai",
    "currencyCode": "INR",
    "currencySymbol": "₹"
  },
  "branding": "jicate",
  "clientName": null,
  "createdDate": "2026-03-18",
  "lastUpdated": "2026-03-18",
  "deployedUrl": "https://jicate-abc-dental-chennai-playbook.vercel.app",
  "status": "deployed",
  "sections": 13,
  "researchSources": 52,
  "companyResearch": {
    "pagesScraped": 12,
    "competitorsAnalyzed": 4,
    "gapsIdentified": 7,
    "maturityPreScore": 2
  },
  "privacy": {
    "publicSourcesOnly": true,
    "robotsTxtRespected": true,
    "disclaimerIncluded": true
  },
  "version": "1.0",
  "versions": [{"version": "1.0", "date": "2026-03-18", "status": "deployed"}]
}
```

## Quality Gate 10: Privacy Compliance (Company Mode Only)

**When:** After Phase 2, before Phase 3
**Check:**
1. Disclaimer text present in Section 1 content
2. No references to login-protected content
3. No personal employee data beyond publicly listed name/title
4. Research JSON has `"dataClassification": "public-sources-only"`
5. Competitor references do not include confidential intelligence
6. `"sourcesAccessed"` array present in Company Intel output

**Fail action:** Remove flagged content, add missing disclaimer
**Max retries:** 1

## Human Review Gate Enhancement (Company Mode)

For company mode, the review prompt includes:

1. "This playbook references [Company Name] **[N] times** and includes **[N] company-specific findings**. Review for accuracy."
2. "Competitor comparison includes **[N] competitors**. Verify no confidential competitive intelligence was used."
3. "Pre-scored maturity at **Level [X]** based on public information. Confirm this is reasonable before sending to [Contact Name]."

## Edge Cases

| Situation | Handling |
|-----------|---------|
| Website is down | Abort: "Cannot reach [url]. Verify URL or try later." |
| Single-page website | Proceed but note limited data. Lean on competitor and industry agents. |
| Non-English website | Attempt scrape (Firecrawl handles many languages). Flag: "Website in [language]. May need additional context." |
| No competitors found | Broaden geography. If still none, use national competitors with note. |
| Very small (1 location, <5 staff) | Skip multi-location references. Emphasize quick wins. |
| Very large (50+ locations) | Escalate complexity. Add enterprise-scale references. |
| No contact provided | Use "leadership team" in hero/CTA. Omit subtitle. |
| Same company, different contact | Allow multiple playbooks. Slug: `abc-dental-chennai-dr-sharma` |
| Recently launched (thin website) | Lean on Industry Context Agent. Frame as "start AI-native from day one." |
| Country not in localization.json | Ask user. Add to localization.json for future use. |
