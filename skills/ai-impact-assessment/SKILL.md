---
name: ai-impact-assessment
description: "Personalized AI Impact Assessment for businesses — analyzes specific industry, workflows, and problems to deliver a tailored AI opportunity report with ROI estimates. Use when a business owner wants to know how AI could impact their specific business, requests an AI readiness assessment, or books through WhatsApp for a personalized AI analysis. Do NOT use for generic AI advice, internal JKKN assessments, or when the client already knows what they want built (use /jicate-prototype instead)."
argument-hint: [new-client|run|deliver|verify|clients|case-study|cancel|archive]
metadata:
  author: JICATE Solutions
  version: 2.0.0
  category: service-delivery
---

# AI Impact Assessment — Full Delivery Skill

You are delivering a personalized AI Impact Assessment that analyzes a business's specific industry, workflows, and problems to produce a tailored report showing exactly where AI can help, with ROI estimates and an implementation roadmap.

**User's command:** `$ARGUMENTS`

---

## ARCHITECTURE — HOW THIS SERVICE WORKS

```
[INPUT]                    [PROCESSING]                      [OUTPUT]
+-----------------------+  +------------------------------+  +---------------------------+
| 1. Business name +    |->| Auto-Research:               |->| AI Impact Assessment      |
|    contact (from      |  |  - WebSearch (company info)   |  | Report (PDF/Markdown)     |
|    operator)          |  |  - Firecrawl (website scrape) |  | via WhatsApp/Email        |
|                       |  |  - Google Maps / JustDial     |  |                           |
| 2. Gap-fill interview |  |  - Social media profiles      |  |                           |
|    (only what auto-   |  | Then: JICATE Framework       |  |                           |
|    research missed)   |  |  mapping + report generation  |  |                           |
+-----------------------+  +------------------------------+  +---------------------------+
```

**What the client gets:** A personalized AI Impact Assessment Report showing 5-10 specific AI opportunities mapped to their business, with ROI estimates, priority ranking, and a recommended implementation roadmap. Plus a 30-minute walkthrough call.

**What we retain (MO):** Assessment data for follow-up, industry benchmarks, and conversion tracking to JICATE paid services.

### Service Type

**Type:** `simple` — No persistent infrastructure. One-shot assessment delivered as a report. Free service used as lead generation for JICATE's paid offerings (₹50K-15L range).

---

## WHAT THE OPERATOR NEEDS TO KNOW

The operator runs 3 commands total:

1. **`/ai-impact-assessment new-client`** — registers the business, captures initial info
2. **`/ai-impact-assessment run {client-id}`** — runs the discovery interview + generates the report
3. **`/ai-impact-assessment verify {client-id}`** — confirms report quality before sending

That's it. The skill handles the rest.

> **Shortcut:** `/ai-impact-assessment deliver {client-id}` runs `run` → `verify` in sequence.

---

## COMMUNICATION RULES

**ALWAYS use plain language.** The operator is a business person, not a developer.

**Positioning:** JICATE delivers RESULTS, not tools. The client never touches AI. They receive a clear report showing where AI saves them time and money. Frame everything as business outcomes.

| DO say | DON'T say |
|--------|-----------|
| "Your personalized AI report" | "Generated artifact" |
| "We analyzed your workflows" | "LLM inference on structured data" |
| "AI opportunity" | "Use case" or "automation vector" |
| "Estimated savings" | "ROI projection model" |
| "Implementation roadmap" | "Technical architecture plan" |
| "Book a walkthrough call" | "Schedule a technical demo" |

When something goes wrong, explain:
1. **What happened** (one sentence)
2. **What to do** (specific action)
3. **Who to call** (if operator can't fix it — "contact the tech team")

---

## KEY TERMS

| Term | Meaning |
|------|---------|
| **Assessment** | The AI Impact Assessment report — the deliverable |
| **Discovery** | The structured interview to understand the client's business |
| **Opportunity** | A specific area where AI can help the client's business |
| **ROI Estimate** | Projected time/money savings from implementing an AI opportunity |
| **Roadmap** | Prioritized sequence of AI implementations recommended |
| **Warm Lead** | A business that scored high engagement — likely to convert to paid JICATE service |
| **JICATE Services** | The 5 paid service lines opportunities map to |
| **Operator** | The JICATE team member running this skill |
| **Client** | The business owner receiving the assessment |

---

## SECURITY REQUIREMENTS

**Data sensitivity:** `low` — Business data, no PII beyond contact details.

### Credential Handling
- Client contact details stored only in assessment records
- No API keys or system credentials involved (this is an advisory service)
- Assessment data treated as confidential business information

### Data Handling
- Assessment data is confidential by default
- **Data retention:** Archive assessment data after 90 days if no conversion to paid service
- Reports may be referenced for case studies only with client permission

---

## COMMAND ROUTING

| Command | Action |
|---------|--------|
| `new-client` | PHASE 0 → Register business, capture initial info |
| `run {client-id}` | PHASE 4 → Discovery interview + report generation |
| `deliver {client-id}` | `run` → `verify` in sequence |
| `verify {client-id}` | PHASE 7 → Quality check + delivery |
| `clients` | Show all assessments' status in one table |
| `case-study {client-id}` | Auto-generate 1-page case study from assessment |
| `cancel {client-id}` | Cancel assessment, archive data |
| `archive {client-id}` | Archive completed assessment |
| _(empty/unclear)_ | Ask what they need in plain language |

---

## PHASE 0: REGISTER BUSINESS

### 0.1 Minimum input from operator

The operator provides ONLY these (from the command or by asking):
- **Business/company name** (required)
- **City/location** (required)
- **Contact person name** (required)
- **Source** — how they found us (optional, e.g., "Yi network", "JCI event", "WhatsApp group")

**DO NOT ask for industry, website, phone, size, or pain points yet.** Auto-research finds these.

### 0.2 Instant Auto-Research (MANDATORY — runs immediately)

The moment you have business name + city, research BEFORE asking anything else:

```
WebSearch: "{company-name} {city}"
WebSearch: "{full-company-name} {city}"
WebSearch: "{company-name} website"
```

From search results, extract:
- **Website URL** → then Firecrawl it: `firecrawl scrape {url} --only-main-content`
- **Industry** (from website/directory descriptions)
- **Services/products** offered
- **Phone/email** (from website contact page or directories)
- **Address** (from Google Maps/JustDial)
- **Size indicators** (from LinkedIn, website, directory)
- **Social media profiles**

Also search directories:
```
WebSearch: "{company-name} {city} JustDial"
WebSearch: "{company-name} {city} LinkedIn"
```

### 0.3 Show auto-research results + ask ONLY gaps

Present what you found:
```
============================================
  AUTO-RESEARCH: {company-name}
============================================

  Found online:
  - Website:     {url}
  - Industry:    {auto-detected}
  - Services:    {list from website}
  - Phone:       {from website/directory}
  - Location:    {address}
  - Size:        {estimate}

  Could NOT find:
  - {list gaps}
============================================
```

Then ask ONLY for what's missing. Typical gaps (can't be found online):
- Phone number (if not on website)
- Specific pain point that prompted them to talk to us
- Referral source (if not provided)

### 0.4 Generate client ID and save

```
Client ID: {kebab-case-company-name}
Date registered: {today}
Source: {referral source}
```

**Collision check:** Verify client ID is unique.

Save assessment record at:
```
Capture/JICATE-Hub/ai-assessments/{client-id}/intake.md
```

Include ALL auto-researched data plus operator-provided gaps.

### 0.5 Show registration summary

```
============================================
  BUSINESS REGISTERED
============================================

  Client ID:     {client-id}
  Business:      {company-name}
  Industry:      {auto-discovered}
  Website:       {url}
  Services:      {from website}
  Contact:       {name} ({phone})
  Location:      {city}
  Source:         {referral-source}

  Auto-research: {N} data points discovered
  Gaps filled:   {N} questions asked

============================================
  Next: /ai-impact-assessment run {client-id}
============================================
```

---

## PHASE 4: AUTO-RESEARCH + DISCOVERY + REPORT GENERATION

Tell the operator: "Running the AI Impact Assessment for {company-name}. I already have their profile from registration. Now I'll deep-dive their industry and ask you just a few things I can't find online."

### 4.1 Deep Industry Research (builds on Phase 0 auto-research)

Phase 0 already captured the company profile. Now research the INDUSTRY context:

```
WebSearch: "{industry} India AI trends 2026"
WebSearch: "{industry} common challenges automation"
WebSearch: "{industry} {city} market size competitors"
WebSearch: "AI in {industry} case studies India"
```

If the website wasn't fully scraped in Phase 0, do a deeper scrape now:
```bash
firecrawl map {website-url}                              # Find all pages
firecrawl scrape {services-url} --only-main-content      # Deep dive services
firecrawl scrape {about-url} --only-main-content         # Company background
```

Also check for competitors and benchmarks:
```
WebSearch: "{industry} {city} companies"
WebSearch: "{company-name} competitors {city}"
```

### 4.2 Inferred Pain Points + Gap-Fill (with SKIP option)

**IMPORTANT:** The operator may not have detailed answers — they may have just met the person briefly at an event. The skill must work with ZERO operator input beyond the business name.

**Step 1 — Generate inferred pain points from research:**

Based on auto-research (industry, company size, services, tech indicators), generate a list of PROBABLE pain points using industry knowledge:

```
============================================
  INFERRED PAIN POINTS: {company-name}
============================================

  Based on {industry} businesses of this size,
  common challenges are:

  1. {Inferred pain point from industry research}
  2. {Inferred pain point from company profile}
  3. {Inferred pain point from service type}
  4. {Inferred pain point from tech indicators}
  5. {Inferred pain point from market context}

  Confidence: HIGH / MEDIUM / LOW
  (based on how much data auto-research found)
============================================
```

**Step 2 — Give the operator 3 options:**

> "Here's what I think their pain points are based on research. You can:"
> 1. **Confirm/modify** — tell me which are right, which are wrong, add any from your conversation
> 2. **Add context** — share anything Senthil mentioned (even one sentence helps)
> 3. **Skip to analysis** — I'll generate the report using research + industry best practices

**If operator picks SKIP:** Proceed directly to 4.3 (Analysis) using:
- All auto-researched company data
- Inferred pain points (clearly marked as "inferred from industry research" in the report)
- Industry benchmarks and best practices
- Generic-but-relevant AI opportunities for that industry

**If operator adds context:** Merge their input with inferred data, then proceed.

The report should clearly label what's from research vs. what's from the client directly:
- "Based on our research..." (inferred)
- "Based on our conversation with {name}..." (confirmed by operator)

**The goal: ALWAYS generate a useful report, even with zero operator input beyond the business name and city.**

### 4.3 Find THE ONE THING (the killer insight)

**This is the most important step in the entire skill. Everything else is scaffolding. This IS the product.**

The goal is NOT a list of 10 AI opportunities. It's finding the ONE specific, irresistible insight that makes this particular business owner say "When can we start?"

**How to find it — the Irresistibility Framework:**

Think like a Nobel laureate studying this specific business. Apply first principles:

**Step 1 — Find the Revenue Bottleneck:**
What is the single constraint that, if removed, would let this business make more money TOMORROW?
- Not "they could use AI for marketing" (generic)
- But "their 12 chemists spend ~3 hours/day writing test reports instead of running tests — that's 36 person-hours/day of ₹500/hr talent doing ₹100/hr work" (specific, quantified, painful)

**Step 2 — Calculate the Cost of Doing Nothing:**
Put a REAL number on the pain. Use their actual data:
- Team size × hours wasted × hourly cost = daily/monthly/yearly cost
- Or: lost revenue because bottleneck limits throughput
- Or: compliance risk exposure (fines, lost accreditation)
- Or: customer churn rate from slow delivery
- The number should make the owner wince. If it doesn't, you haven't found the right thing.

**Step 3 — The Flip:**
Show what happens when it's solved:
- "Your 12 chemists get back 36 hours/day → that's 180 more tests/month → at ₹2,000/test average, that's ₹3.6L/month additional capacity you're currently leaving on the table"
- The flip should feel like found money, not a cost

**Step 4 — Make it Tangible:**
Describe the solution in ONE sentence a 10-year-old could understand:
- "AI reads your instrument data and writes the first draft of every test report — your chemists just review and approve"
- Not: "We'll implement an AI-powered LIMS integration with automated report generation"

**Step 5 — The Proof:**
Find a real example of a similar business that solved this:
```
WebSearch: "AI {their specific bottleneck} {their industry} case study"
WebSearch: "lab automation report generation AI India"
WebSearch: "{similar company} digital transformation results"
```

**Step 6 — The Irresistible Offer:**
Structure it so saying "no" feels irrational:
- "We'll build a working prototype in 48 hours"
- "You'll see it working on YOUR data before you pay anything"
- "If it doesn't save at least X hours in the first month, you owe us nothing"

### 4.4 Generate the Report (Led by THE ONE THING)

Create the report at:
```
Capture/JICATE-Hub/ai-assessments/{client-id}/ai-impact-report.md
```

**Report Structure — THE ONE THING leads everything:**

```markdown
# {Company Name} — What AI Can Do For You
### A personalized assessment by JICATE Solutions

---

## The One Thing That Changes Everything

{THE KILLER INSIGHT — 3-4 sentences that hit like a punch.}

{One sentence showing the cost of doing nothing — a number that hurts.}

{One sentence showing the flip — what life looks like when it's solved.}

{One sentence making it tangible — what actually happens, in plain language.}

---

## The Numbers

| Today | With AI |
|-------|---------|
| {Current painful reality — specific metric} | {Transformed state — specific metric} |
| {Hours wasted on X} | {Hours freed for Y} |
| {Revenue left on table} | {Revenue unlocked} |
| {Risk/compliance exposure} | {Risk eliminated} |

**Net impact:** {Annual savings or revenue gain — the big number}

---

## How It Works (Simple Version)

{3-step explanation a non-technical business owner can picture:}

1. **{Step 1}** — {what happens, in their language}
2. **{Step 2}** — {what changes, visually describable}
3. **{Step 3}** — {the result they care about}

{If a proof/case study was found:}
> **Real example:** {Similar company} implemented this and saw {specific result}.

---

## What Else We Found

While analyzing your business, we also spotted these opportunities:

| Opportunity | Impact | Effort | Investment |
|------------|--------|--------|------------|
| {#2 opportunity — one line} | {savings} | {timeline} | {cost} |
| {#3 opportunity — one line} | {savings} | {timeline} | {cost} |
| {#4 opportunity — one line} | {savings} | {timeline} | {cost} |

These are listed for completeness. But start with The One Thing — everything else compounds from there.

---

## Your Business Profile

| | |
|---|---|
| **Company** | {name} |
| **Industry** | {specific sub-industry} |
| **Team** | {size and composition} |
| **Accreditations** | {if any} |
| **Serving** | {customer types} |
| **Online Presence** | {website quality + social} |

---

## Next Step — Just One

**See it working on YOUR data. Free. 48 hours.**

We'll build a working prototype of The One Thing using your actual {data type}.
If it doesn't deliver, you owe us nothing.

{Operator's name} | {phone} | {WhatsApp link}

---

*Prepared specifically for {Company Name}. Not a template — this analysis used
{N} data points from your website, industry research, and {source context}.*
```

### 4.5 Verify report quality

Before proceeding, self-check:
- [ ] **THE ONE THING is genuinely specific** — mentions their company name, team size, specific workflow. If you could swap in another company name and it still reads the same, it's not specific enough. Redo it.
- [ ] **The cost number hurts** — it should make the owner think "I didn't realize it was THAT much"
- [ ] **The flip feels like found money** — not a cost, not an investment, but money they're currently throwing away
- [ ] **A 10-year-old could understand the solution** — no jargon, no acronyms
- [ ] **The CTA is one step, not a menu** — "See it working on your data" not "pick from these 5 options"
- [ ] **Proof exists** — a real case study or benchmark, not a hypothetical

Tell the operator: "Assessment complete for {company-name}. Found {N} AI opportunities with estimated combined savings of {amount}. Ready for review."

OR if the discovery didn't yield enough: "I need more information about {specific area}. Can you ask the client about {question}?"

---

## PHASE 7: END-TO-END VERIFICATION & DELIVERY

### 7.1 Quality check

| Step | Check | Expected |
|------|-------|----------|
| Report exists | File at assessments/{client-id}/ai-impact-report.md | Present |
| Opportunities found | At least 3 specific opportunities | 3-10 |
| ROI estimates | Each opportunity has savings estimate | All present |
| Business-specific | References client's actual workflows | Not generic |
| JICATE mapping | Opportunities link to service lines | Mapped |

### 7.2 Delivery report

```
============================================
  ASSESSMENT COMPLETE
============================================
  Business:     {company-name}
  Industry:     {industry}

  Opportunities Found: {N}
  Est. Combined Savings: {amount}/year
  Top Priority: {#1 opportunity name}

  Report Location:
  Capture/JICATE-Hub/ai-assessments/{client-id}/

  NEXT STEPS:
  1. Review the report
  2. Send to client via WhatsApp
  3. Schedule 30-min walkthrough call
  4. Track conversion in JICATE Hub
============================================
```

### 7.3 Handover

1. Present the report to the operator for review
2. **Operator sends report to client** via WhatsApp (PDF) or Email
   - IMPORTANT: Per CLAUDE.md, NEVER auto-send WhatsApp. Operator must send manually.
3. Operator schedules a 30-minute walkthrough call
4. After walkthrough, track if client converts to any JICATE paid service
5. Save conversion status to assessment record

> "Assessment delivered to {company-name}. Report saved. Schedule the walkthrough call to discuss opportunities."

---

## OPERATOR QUICK REFERENCE

| Command | What it does |
|---------|-------------|
| `/ai-impact-assessment new-client` | Register a new business |
| `/ai-impact-assessment run {id}` | Discovery interview + report generation |
| `/ai-impact-assessment deliver {id}` | Run + verify in one go |
| `/ai-impact-assessment verify {id}` | Quality check report |
| `/ai-impact-assessment clients` | All assessments status |
| `/ai-impact-assessment case-study {id}` | Auto-generate case study |
| `/ai-impact-assessment cancel {id}` | Cancel and archive |
| `/ai-impact-assessment archive {id}` | Archive completed assessment |

---

## `/ai-impact-assessment case-study {client-id}` — Auto-Generate Case Study

```markdown
# Case Study: {client_name}

## Client Profile
- **Industry:** {industry}
- **Size:** {employees} employees
- **Location:** {city}

## Challenge
{client_name} needed to understand where AI could make a real difference in their {industry} business, particularly around {top pain point}.

## Our Approach
JICATE Solutions conducted a personalized AI Impact Assessment — analyzing their specific workflows, technology stack, and growth goals to identify concrete AI opportunities.

## Key Findings
- **{N} AI opportunities** identified across {departments}
- **Top opportunity:** {#1 opportunity} with estimated savings of {amount}/year
- **Quick wins:** {2-3 immediate actions}

## Result
{client_name} chose to implement {which opportunity}, leading to {outcome if known, or "walkthrough scheduled for implementation planning"}.

## Client Testimonial
> "[Placeholder — request from client]"
```

Save to vault and tell operator to customize before sharing.

---

## `/ai-impact-assessment clients` — Assessment Monitor

```
Business Name       Industry         Date           Status        Conversion
-----------         --------         ----           ------        ----------
ABC Textiles        Manufacturing    2026-03-15     DELIVERED     Pending
XYZ Builders        Construction     2026-03-12     CONVERTED     Software (₹3.5L)
PQR Hospital        Healthcare       2026-03-10     DELIVERED     Follow-up scheduled
```

**Status logic:**
- **NEW** = registered, interview not done
- **IN PROGRESS** = interview done, report being generated
- **DELIVERED** = report sent to client
- **CONVERTED** = client purchased a JICATE paid service
- **ARCHIVED** = 90 days passed, no conversion

---

## CANCELLATION & FAILURE PROTOCOL

### `/ai-impact-assessment cancel {client-id}`

1. Confirm with operator: "This will cancel the assessment for {company-name}. Proceed?"
2. Archive assessment files
3. Update status to `cancelled`
4. Tell operator: "Assessment cancelled. Data archived."

### Delivery Failure Handling

If the discovery interview doesn't yield enough information:
1. Tell operator which areas need more detail
2. Provide specific follow-up questions to ask the client
3. Resume when information is available
4. Never generate a generic report — better to ask more questions

---

## KNOWN EDGE CASES

### Service-specific
- **Paper-based businesses:** Many SMEs in Erode-Tirupur still run on paper. Frame AI as "making your existing process faster" not "replacing your system"
- **Language barrier:** Some clients prefer Tamil. Operator should translate key findings. Report stays in English but walkthrough can be bilingual
- **"We're too small for AI":** Counter with Quick Wins under ₹1L. Show that AI starts small
- **Competitor fear:** Some clients worry about data security. Emphasize JICATE's no-client-side-installation model
- **Multiple decision makers:** In family businesses, the person you interview may not be the decision maker. Ask "who else should see this report?"

### Conversion optimization
- Fastest conversion: clients who came via WhatsApp group (warm leads)
- Slowest conversion: cold prospects (need multiple touchpoints)
- Best conversion: manufacturing + textile businesses (clear ROI on process automation)
- Partner discount (50%) available for Yi members, JKKN alumni companies

---

## EXAMPLES

**First-time delivery:**
```
User: /ai-impact-assessment new-client
Skill: "What's the business name?" → "Industry?" → "Contact?" → registers
Skill: "Business registered. ID: raagam-exports. Next: /ai-impact-assessment run raagam-exports"

User: /ai-impact-assessment deliver raagam-exports
Skill: Asks 5 batches of discovery questions → generates report → verifies quality
Skill: "Assessment complete. 7 AI opportunities found. Estimated ₹12L/year savings. Report ready to send."
```

**Check all assessments:**
```
User: /ai-impact-assessment clients
Skill: Shows table of all businesses, their status, and conversion tracking
```

---

## REFERENCE FILES

See the `references/` directory:
- [Operator Playbook](references/operator-playbook.md) — Printable step-by-step checklist
- [Onboarding Form](references/onboarding-form.md) — Client intake questionnaire
- [Quotation](references/quotation.md) — Pricing for follow-up paid services
- [SOW](references/sow.md) — Statement of work template
- [Delivery Certificate](references/delivery-certificate.md) — Sign-off document
- [Known Fixes](references/known-fixes.md) — Issues found and fixed across deployments
