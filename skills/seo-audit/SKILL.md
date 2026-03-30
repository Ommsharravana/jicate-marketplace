---
name: seo-audit
description: "SEO+GEO+AEO audit service: website health reports with AI visibility analysis, competitor benchmarking, and action plans. Triggers on 'audit this site', 'SEO report for', 'check AI visibility', 'competitor analysis', 'website health'. NOT for general SEO advice, content writing, paid ads, social media, or link building."
argument-hint: [new-client|run|deliver|status|debug|verify|clients|analyze|case-study|alerts|cancel|archive|revoke-link|delete-client|prep-machine]
metadata:
  author: JICATE Solutions
  version: 1.0.0
  category: service-delivery
---

# SEO+GEO+AEO Audit — Full Delivery Skill

You are delivering a comprehensive SEO+GEO+AEO audit for a client. This combines traditional SEO scoring, Generative Engine Optimization (AI-readiness), Answer Engine Optimization (AI citation testing), competitor benchmarking, and a prioritized action plan — all in one report.

**User's command:** `$ARGUMENTS`

---

## ARCHITECTURE

```
[INPUT]                         [PROCESSING]                        [OUTPUT]
+-----------------------+      +---------------------------+       +-------------------------+
| Client URL(s)         |----->| Firecrawl (content +      |------>| SEO+GEO+AEO Report      |
| GSC data (optional)   |      |   site structure)          |       | + Prioritized Action Plan|
| Target queries        |      | PageSpeed Insights (scores)|       | + Competitor Matrix      |
| Known competitors     |      | AI Models (GEO/AEO)       |       |                          |
+-----------------------+      | Claude (analysis +         |       | Delivery: WhatsApp >     |
                               |   recommendations)         |       | Email > Portal           |
                               +---------------------------+       +-------------------------+
```

**What the client gets:** Professional PDF report with overall digital health score (0-100), per-site scorecards, competitor comparison matrix, and a prioritized action plan with specific fix instructions.

**What we retain (MO):** Admin dashboard access, historical score data, automated monitoring config, cron-triggered re-audits.

### Service Type

**Type:** `standard` — orchestrates 6+ external systems (Firecrawl, PSI, GSC, ChatGPT, Gemini, Perplexity), requires per-client OAuth setup, manages Firecrawl credit budgets, and stores persistent client configs.

**One skill, two modes:**
- **PaP** (Pipeline as Product) — one-time audit. Client gets report + action plan. Done.
- **MO** (Managed Operations) — monthly retainer. Automated re-audits, dashboard, trend tracking, fix execution, alerting.

The `delivery_model` field in client config (`pap` or `mo`) determines which features activate.

### Implementation Status

| Feature | Status | Build Phase |
|---------|--------|-------------|
| PaP full pipeline (crawl, score, GEO/AEO, report) | **Ready** | Phase A |
| PDF generation (WeasyPrint + branding) | **Not yet built** | Phase B |
| Supabase schema + sync | **Not yet built** | Phase C |
| Dashboard (Next.js on Vercel) | **Not yet built** | Phase C/D |
| Cron automation (n8n / launchd) | **Not yet built** | Phase E |
| Automated re-audits | **Not yet built** | Phase E |
| v2.0 commands (analyze, case-study) | **Not yet built** | v2.0 |

**Phase A is the only blocker for PaP delivery.** All other phases can be built incrementally after the first successful PaP audit. MO clients can be served manually (operator runs monthly) until Phases C-E are built.

---

## WHAT THE OPERATOR NEEDS TO KNOW

The operator runs 4 commands total:

1. **`/seo-audit new-client`** — provisions the client, stores config
2. **`/seo-audit run --client {slug}`** — executes the full audit pipeline
3. **`/seo-audit deliver --client {slug}`** — generates PDF, shows path for manual send
4. **`/seo-audit verify --client {slug}`** — proves end-to-end delivery

That's it. The skill handles the rest.

> **First-time flow:** `new-client` -> `run` -> `deliver` -> `verify`
> **MO monthly flow:** `run` -> `deliver` (automated in Phase E)

---

## COMMUNICATION RULES

**ALWAYS use plain language.** The operator is a business person, not a developer.

**Positioning:** JICATE delivers RESULTS, not tools. The client never touches AI. They receive a report.

| DO say | DON'T say |
|--------|-----------|
| "Here's your SEO health report" | "Generated audit artifact" |
| "Your digital health score is 72 out of 100" | "Lighthouse returned a 72 performance metric" |
| "AI models don't mention your site — here's how to fix that" | "GEO citation frequency is suboptimal" |
| "Your competitor ranks higher for these searches" | "SERP position delta is negative" |
| "Client account" | "Supabase tenant" |

When something goes wrong:
1. **What happened** (one sentence)
2. **What to do** (specific action)
3. **Who to call** (if operator can't fix it — "contact the tech team")

---

## KEY TERMS

| Term | Meaning |
|------|---------|
| **PaP** | Pipeline as Product — one-time delivery. Client gets the report and manages improvements themselves. |
| **MO** | Managed Operations — monthly retainer. JICATE manages ongoing audits, dashboard, and fix execution. |
| **SEO** | Search Engine Optimization — how well the site ranks in Google. |
| **GEO** | Generative Engine Optimization — how AI-ready the content is (structured data, citation-worthiness). |
| **AEO** | Answer Engine Optimization — whether AI models (ChatGPT, Gemini, Perplexity) actually cite the client. |
| **Gate** | A checkpoint that MUST pass before the next phase begins. If a gate fails, STOP and tell the operator. |
| **Operator** | The JICATE team member running this skill. |
| **Client** | The business receiving the SEO audit. |
| **Basic tier** | URL-only audit. No GSC/GA access needed. |
| **Premium tier** | URL + GSC + GA access. Deeper analysis with real search data. |

---

## SECURITY REQUIREMENTS

**Data sensitivity:** `low` — business website data, no PII processed. Public website content + aggregated search metrics.

### Credential Handling
- **NEVER** store API keys in skill files, git, or client-visible documents
- All API keys via env vars: `GEMINI_API_KEY`, `PERPLEXITY_API_KEY`, `OPENAI_API_KEY`, `PSI_API_KEY`, `FIRECRAWL_API_KEY`
- GSC OAuth tokens: macOS Keychain (PaP) or Supabase Vault (MO). **NOT in plain JSON.**
- Operator credentials: already configured via Claude Code global auth

### Data Handling
- Client data confidential by default
- **Data retention:** PaP: 90 days post-delivery. MO: contract duration + 90 days.
- **Data isolation:** All Supabase queries MUST filter by `client_id`
- **Deletion:** `/seo-audit delete-client` purges: JSON config, Supabase rows, Storage files, vault markdown, share links

### Competitor Crawling Policy
- Always check and respect competitor `robots.txt` before crawling
- **NEVER** use `proxy: "stealth"` on competitor sites
- If competitor blocks crawling, use SERP-only data and note limitation

---

## COMMAND ROUTING

| Command | Action |
|---------|--------|
| `prep-machine` | PHASE -1 -> Check and install prerequisites |
| `new-client` | PHASE 0 -> Provision client -> Store config |
| `run --client {slug}` | PHASE 2 -> Execute full audit pipeline |
| `run --client {slug} --site {url}` | PHASE 2 -> Audit one specific URL from client's list |
| `run --client all` | PHASE 2 -> Run for all active clients (MO batch) |
| `deliver --client {slug}` | PHASE 3 -> Generate PDF -> Show path for manual send |
| `status --client {slug}` | PHASE 4 -> Health report |
| `debug --client {slug}` | PHASE 5 -> Diagnose issues |
| `verify --client {slug}` | PHASE 6 -> End-to-end proof |
| `clients` | Show all clients' status in one table |
| `alerts` | Check for stale/failed deliveries across all clients |
| `cancel --client {slug}` | Graceful cancellation -> archive data -> revoke access |
| `archive --client {slug}` | Archive completed/cancelled client |
| `revoke-link --client {slug}` | Revoke active share link for client |
| `delete-client --client {slug}` | Purge ALL client data (requires confirmation) |
| _(empty/unclear)_ | Ask what they need in plain language |
| `analyze --client {slug}` | (v2.0 — not yet available) Generate value-add insights report |
| `case-study --client {slug}` | (v2.0 — not yet available) Auto-generate 1-page case study |

**Note on `new-client` and `prep-machine`:** These commands take no `--client` flag. All other commands require `--client {slug}`.

**Phase flow:**
- **Basic tier:** Phase 0 (provision) -> Phase 2 (execute) -> Phase 3 (deliver) -> Phase 6 (verify)
- **Premium tier:** Phase 0 (provision) -> Phase 1 (GSC OAuth) -> Phase 2 (execute) -> Phase 3 (deliver) -> Phase 6 (verify)

---

## PHASE -1: ENVIRONMENT PREP

### -1.1 Check prerequisites

Verify the operator's machine has:
- [ ] Firecrawl MCP configured (check `FIRECRAWL_API_KEY` env var)
- [ ] AI API keys set: `GEMINI_API_KEY`, `PERPLEXITY_API_KEY`, `OPENAI_API_KEY`
- [ ] PageSpeed Insights API key: `PSI_API_KEY` (enable PSI API in GCP project)
- [ ] WeasyPrint installed (for PDF generation): `pip install weasyprint`
- [ ] Client config directory exists: `~/.claude/skills/seo-audit/clients/`

### -1.2 Install missing dependencies

```bash
# Check all env vars
for key in FIRECRAWL_API_KEY GEMINI_API_KEY PERPLEXITY_API_KEY OPENAI_API_KEY PSI_API_KEY; do
  if [ -z "${!key}" ]; then echo "MISSING: $key"; else echo "OK: $key"; fi
done

# Install WeasyPrint if needed
pip install weasyprint 2>/dev/null || pip3 install weasyprint

# Verify Firecrawl MCP
echo "Check .mcp.json for firecrawl configuration"
```

### -1.3 Verify environment

**GATE: If any API key is missing, STOP.** Tell the operator which key is needed and where to get it:
- Gemini: Google AI Studio (free tier)
- Perplexity: Perplexity dashboard (~$1/1000 queries)
- OpenAI: OpenAI platform (gpt-4o-mini, ~$0.15/1000 queries)
- PSI: Google Cloud Console (free, enable PageSpeed Insights API)

> "Environment ready. All API keys configured."

---

## PHASE 0: PROVISION CLIENT

### 0.1 Collect client information

Ask the operator:
- **Client/company name?**
- **Website URL(s)?** (or empty for competitor-only audit)
- **Industry?** (auto-detected from site content if not specified)
- **Known competitors?** (default: 3, max 5)
- **Target search queries?** (what should this site rank for?)
- **Service tier: Basic or Premium?**
- **Delivery model: PaP (one-time) or MO (monthly)?**
- **Report audiences?** (business-owner / marketing / it — which sections to include)
- **GSC access available?** (Premium tier only)
- **Delivery channel: WhatsApp or Email?**
- **Regulated industry?** (pharma, finance, healthcare — adds compliance disclaimer)

### 0.2 Generate client slug and config

```
Client slug: lowercase-hyphenated-name (e.g., "jkkn-institutions")
Config file: ~/.claude/skills/seo-audit/clients/{slug}.json
```

**Slug validation:** Regex: `^[a-z0-9][a-z0-9-]{1,48}[a-z0-9]$` (3-50 chars, lowercase alphanumeric + hyphens, no leading/trailing hyphen). Reject any slug containing `/`, `\`, `..`, `~`, space, or shell metacharacters. Collision check: verify slug does not already exist.

**Provisioning validation gate:** At least one URL or one competitor must be provided. If both `urls` and `competitors.known` are empty, reject with: "At least one URL or one competitor is required."

**URL validation:** Each URL must:
- Start with `https://`
- Resolve to a public IP
- Reject private/reserved ranges: `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`, `169.254.0.0/16`, `127.0.0.0/8`, `0.0.0.0/8`, IPv6 `fc00::/7`, `fe80::/10`, `::1/128`
- Reject `localhost` and URL-encoded IP bypasses
- Reject URLs with embedded credentials (`user:pass@`)
- Normalize: strip trailing slash, lowercase domain
- **Max URLs:** 10 per client (Firecrawl budget constraint on Hobby plan). Warn if >5.

**Target query validation:** If zero target queries provided, auto-generate from crawled content (page titles, meta descriptions) during Phase 2 Step 1. If auto-generation also produces nothing, set GEO/AEO to "N/A" in the report.

### 0.3 Store client config

Save JSON config per the schema in `references/client-config-schema.md`.

For MO clients: also insert into Supabase `clients` table (when Phase C is built).

### 0.4 Show confirmation

```
============================================
  CLIENT READY
============================================

  Client:     {client_name}
  Slug:       {slug}
  URLs:       {url_count} site(s)
  Tier:       {service_tier}
  Model:      {delivery_model}
  Competitors: {competitor_count}
  Queries:    {query_count}

============================================
  Next: /seo-audit run --client {slug}
============================================
```

---

## PHASE 1: CONFIGURE (Premium tier — GSC OAuth)

For Premium tier clients only. Basic tier clients skip this phase.

### 1.1 GSC OAuth setup

Read `references/gsc-oauth-setup.md` for the full OAuth flow.

Summary:
1. Operator sends client a browser URL for consent
2. Client grants read-only GSC access
3. Callback receives auth code
4. Skill exchanges for tokens
5. Tokens stored securely (macOS Keychain for PaP, Supabase Vault for MO)

### 1.2 Verify GSC connection

**GATE: If GSC token is invalid or expired, STOP.** Tell operator to re-authorize.

> "GSC connected. Found {property_count} properties with {total_clicks} clicks in the last 28 days."

---

## PHASE 2: EXECUTE AUDIT PIPELINE

Tell the operator: "Now running the full SEO+GEO+AEO audit. Single site (Basic): ~30-60 minutes. Single site (Premium): ~60-90 minutes. Multi-site (10 URLs): ~3-4 hours."

The pipeline has 7 steps. Each step has error boundaries — the pipeline never halts silently. Every failure is logged and reflected in the report as a data gap.

### Step 1: CRAWL — Firecrawl map + scrape per site

For each client URL:
1. **`firecrawl_map`** — discover all pages on the site
2. **Smart sampling** — select top pages to audit (homepage, top landing pages, program pages). For sites with 1000+ pages, sample top 50 by estimated traffic.
3. **`firecrawl_scrape`** with `formats: ["markdown"], onlyMainContent: true` — extract content, meta tags, headings, structured data, images, internal links
4. For SPAs: add `waitFor: 5000` to scrape calls

**On failure:** Log error, mark site as "crawl-failed", continue to next site.
**On Firecrawl down:** HALT pipeline, notify operator, suggest retry later.

### Step 2: SCORE — PageSpeed Insights per site

For each client URL (homepage + top landing pages):
1. Call PSI API: `https://www.googleapis.com/pagespeedonline/v5/runPagespeed?url={url}&key={PSI_API_KEY}&strategy=mobile`
2. Repeat with `strategy=desktop`
3. Extract: Performance, Accessibility, Best Practices, SEO scores, Core Web Vitals (LCP, INP, CLS)

**On failure:** Retry once. If still failing, mark "score-unavailable".

### Step 3: GEO TEST — AI model queries

For each target query, generate 3 phrasings and run each 3 times per model (= 9 runs per query per model):

| Model | API | Settings |
|-------|-----|----------|
| Gemini | `gemini-2.0-flash` | `temperature: 0` |
| Perplexity | sonar model | Returns citations natively |
| ChatGPT | `gpt-4o-mini` with web_search tool | `temperature: 0` |

Record for each run:
- Does the AI mention the client? What does it say?
- What competitors does it cite instead?
- Any hallucinations (positive or negative)?
- Model version used

**On partial failure (1-2 models down):** Continue with available models, note gaps.
**On full failure (all down):** Skip GEO/AEO section, deliver SEO-only report with disclaimer.

### Step 4: AEO TEST — SERP analysis

For each target query:
1. Use WebSearch to check: featured snippets, People Also Ask, Knowledge Panel
2. Record SERP positions for client and competitors
3. Identify queries where client is absent but competitors rank

### Step 5: COMPETITOR BENCHMARK

For each competitor (default 3, max 5):
1. **Check robots.txt** — respect crawl restrictions
2. Lightweight crawl: homepage + key landing pages only (NOT full audit)
3. Run same PSI and GEO/AEO tests (reduced scope)
4. Side-by-side comparison: client vs each competitor

**On competitor crawl blocked:** Use SERP-only data, note limitation.

### Step 6: ANALYZE — Claude synthesizes all data

Claude analyzes all collected data and generates:
- Overall Digital Health Score (0-100, see scoring in `references/scoring-methodology.md`)
- Per-site scorecards with category scores
- Top 3 wins (highest-scoring categories)
- Top 5 critical fixes (lowest-scoring with highest potential impact)
- Industry-specific recommendations (detected from site content, not hardcoded)
- Prioritized action plan (P1: quick wins, P2: this month, P3: next quarter)
- Competitor position analysis

### Step 7: GENERATE REPORT

Produce markdown report following the template in `references/report-template.md`.

Report adapts by client type:
- **Multi-site (2+ URLs):** All 5 sections (exec summary, per-site scorecards, competitor matrix, action plan, tech appendix)
- **Single-site (1 URL):** Skip "Cross-site insights", scorecard becomes main body
- **No-website (competitor-only):** Competitor matrix + "build your site like this" blueprint

### Step 8: Verify pipeline execution

Tell the operator: "Audit complete. Overall score: {score}/100 ({grade}). Report saved to vault." or "Partial audit — {what_failed}. Report generated with data gaps noted."

---

## PHASE 3: DELIVER

Generate the PDF report from the markdown artifact and present the file path to the operator for manual delivery.

1. Convert markdown report to PDF via WeasyPrint (with JICATE branding)
2. Save PDF to: `~/Downloads/{slug}-seo-audit-{date}.pdf`
3. If WeasyPrint fails: fall back to `pandoc` or offer the markdown file directly. Never block delivery on PDF generation failure.
4. Generate Delivery Certificate (see `references/delivery-certificate.md`) with all fields pre-filled from audit data
5. Tell operator: "PDF ready at {path}. Send to client via {delivery_channel}."

**If `last_audit` is null (no audit has been run):** STOP. Tell operator: "No audit data found. Run the audit first: `/seo-audit run --client {slug}`."

---

## PHASE 4: HEALTH REPORT

```
============================================
  SEO AUDIT — HEALTH REPORT
============================================
  Client:            {client_name}
  Tier:              {service_tier}
  Model:             {delivery_model}
  Status:            OK / WARNING / ERROR

  Last audit:        {date}
  Overall score:     {score}/100 ({grade})

  Category scores:
  - SEO:             {seo_score}/100
  - GEO:             {geo_score}/100
  - AEO:             {aeo_score}/100
  - Speed:           {speed_score}/100
  - Mobile:          {mobile_score}/100

  Sites audited:     {count}
  Competitors:       {count}
  Action items:      {pending_count} pending
============================================
```

---

## PHASE 5: DEBUG & DIAGNOSE

Run checks and explain in plain language:

```
Problem: Firecrawl credits exhausted
Fix: Check remaining credits with `firecrawl --status`. Upgrade plan or wait for monthly reset.

Problem: PSI API returning errors
Fix: Verify PSI_API_KEY is valid. Check if PageSpeed Insights API is enabled in GCP console.

Problem: AI model API not responding
Fix: Check API key validity. Try a manual test query. If persistent, deliver SEO-only report.

Problem: GSC token expired
Fix: Re-run OAuth flow. Have client re-authorize read-only access.

Problem: Client site is down
Fix: Confirm with client. Report "site unreachable" as critical finding.

Problem: Can't figure it out.
Fix: Contact the tech team with: client name, what step failed, screenshot.
```

---

## PHASE 6: END-TO-END VERIFICATION

### 7.1 Pipeline check

| Step | Check | Expected |
|------|-------|----------|
| Crawl completed | All client URLs scraped | Content extracted, no "crawl-failed" sites |
| Scores generated | PSI data for all sites | Performance, SEO, a11y scores present |
| GEO/AEO tested | AI model queries ran | Citation data for at least 2 of 3 models |
| Competitors benchmarked | SERP + lightweight analysis | Comparison data for 3+ competitors |
| Report generated | Markdown report exists | All sections filled, no placeholder text |
| PDF generated | WeasyPrint output | File exists, opens correctly, branding applied |

### 7.2 Delivery report

```
============================================
  DELIVERY COMPLETE
============================================
  Client:     {client_name}
  Score:      {overall_score}/100 ({grade})

  Report:     {vault_path}
  PDF:        {pdf_path}

  Key findings:
  - Top win: {top_win}
  - Critical fix: {top_fix}
  - vs Competitors: {position_summary}

  NEXT STEP:
  Send PDF to client via {delivery_channel}.
============================================
```

### 7.3 Handover — PaP Mode

1. Present delivery report
2. Tell operator: "PDF ready at {path}. Send to client via WhatsApp or email."
3. **Per CLAUDE.md: NEVER auto-send WhatsApp without explicit permission each time**
4. Inform client: 90-day support period included
5. After support period: revoke GSC access, archive client data

### 7.4 Handover — MO Mode

1. Present delivery report + dashboard link
2. Share view-only dashboard URL with client (expiring share link, 30 days)
3. Configure monthly re-audit schedule
4. Operator retains admin access; client gets view-only

---

## OPERATOR QUICK REFERENCE

### Delivery commands
| Command | What it does |
|---------|-------------|
| `/seo-audit prep-machine` | Check/install prerequisites (first-time only) |
| `/seo-audit new-client` | Provision new client account |
| `/seo-audit run --client {slug}` | Execute the full audit pipeline |
| `/seo-audit deliver --client {slug}` | Generate PDF, show path for sending |
| `/seo-audit status --client {slug}` | Check if everything works |
| `/seo-audit debug --client {slug}` | Find and fix problems |
| `/seo-audit verify --client {slug}` | Prove end-to-end delivery |

### Management commands
| Command | What it does |
|---------|-------------|
| `/seo-audit clients` | All clients' status in one table |
| `/seo-audit alerts` | Check for stale/failed deliveries |
| `/seo-audit revoke-link --client {slug}` | Revoke client's share link |
| `/seo-audit cancel --client {slug}` | Cancel service, revoke access |
| `/seo-audit delete-client --client {slug}` | Purge ALL client data |
| `/seo-audit archive --client {slug}` | Archive completed client |

---

## `/seo-audit clients` — Multi-Client Monitor

```
Client Name       Score  Last Audit         Status     Tier     Model
-----------       -----  ----------         ------     ----     -----
JKKN Inst.        72     2026-03-15         OK         Premium  MO
Tirupur Tex       58     2026-03-10         STALE      Basic    PaP
Salem Hosp        81     2026-03-05         OK         Premium  MO
```

---

## CANCELLATION & FAILURE PROTOCOL

### `/seo-audit cancel --client {slug}`

1. Confirm with operator: "This will stop all service for {client_name}. Proceed?"
2. Revoke all access: GSC tokens, share links, dashboard credentials
3. Export client data to archive: `Capture/JICATE-Hub/seo-audit-archive-{slug}/`
4. Update config: mark as `cancelled`
5. Tell operator: "Client cancelled. Data archived. All access revoked."

### `/seo-audit delete-client --client {slug}`

**Destructive — requires confirmation:** Purges JSON config, Supabase rows, Storage files, vault markdown, share links.

### Delivery Failure Handling

If any phase fails irrecoverably:
1. **Document** what failed (add to `references/known-fixes.md`)
2. **Notify** operator with plain-language explanation
3. **Preserve** all client data and partial work
4. **Degradation rule:** A report with partial data and clear disclaimers is ALWAYS better than no report

---

## KNOWN EDGE CASES

### Service-specific
- **Site behind login/paywall:** Skip authenticated pages. Audit public-facing pages only.
- **Client has no website:** Offer competitor-only audit with "build your site like this" blueprint.
- **SPA (React/Angular/Vue):** Use Firecrawl with `waitFor: 5000-10000ms`. Flag client-side rendering impact.
- **Regulated industry (pharma, finance, healthcare):** Auto-include regulatory disclaimer.
- **Client is market leader (already #1):** Reframe: "threats to your position" instead of "how to catch up."
- **AI model returns positive hallucination:** Flag: "AI currently overstates [claim]. May be corrected in future updates."
- **All AI models unavailable:** Deliver SEO-only report with clear disclaimer.
- **Firecrawl credits running low:** Warn operator before starting. Prioritize client sites over competitors.

### Infrastructure
- Firecrawl Hobby plan: 3000 credits/month. Budget ~400-500 per single-site audit, ~1300-2300 per 10-site audit.
- PSI API: Free, 60 queries/min. Pace bulk operations.
- All Supabase queries MUST include `client_id` — multi-tenant isolation.
- Dashboard timezone: IST (Asia/Kolkata).

---

## EXAMPLES

**First-time delivery (PaP):**
```
User: /seo-audit new-client
Skill: "What's the client name?" -> "Website URLs?" -> "Basic or Premium?" -> provisions client
Skill: "Client ready. Slug: acme-textiles. Next: /seo-audit run --client acme-textiles"

User: /seo-audit run --client acme-textiles
Skill: Crawls site -> Scores pages -> Tests AI models -> Benchmarks competitors -> Generates report
Skill: "Audit complete. Overall score: 58/100 (D). Report saved. Next: /seo-audit deliver --client acme-textiles"

User: /seo-audit deliver --client acme-textiles
Skill: Generates PDF -> "PDF ready at ~/Downloads/acme-textiles-seo-audit-2026-03-15.pdf. Send to client via WhatsApp."
```

**Check on a client:**
```
User: /seo-audit status --client jkkn-institutions
Skill: Shows health report with scores, last audit date, pending actions
```

---

## REFERENCE FILES

See the `references/` directory:
- [Scoring Methodology](references/scoring-methodology.md) — How 0-100 scores are calculated
- [Report Template](references/report-template.md) — Full report structure and content rules
- [Client Config Schema](references/client-config-schema.md) — JSON config format
- [GSC OAuth Setup](references/gsc-oauth-setup.md) — Step-by-step OAuth flow for Premium clients
- [Operator Playbook](references/operator-playbook.md) — Printable step-by-step checklist
- [SOW](references/sow.md) — Statement of work for client signature
- [Delivery Certificate](references/delivery-certificate.md) — Sign-off document
- [Quotation](references/quotation.md) — Pricing for PaP/MO/Basic/Premium
- [Known Fixes](references/known-fixes.md) — Issues found and fixed across deployments
- [Onboarding Form](references/onboarding-form.md) — Client intake questionnaire
