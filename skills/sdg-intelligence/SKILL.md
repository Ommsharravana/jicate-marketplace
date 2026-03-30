---
name: sdg-intelligence
description: "AI-powered SDG mapping and accreditation report generation for Higher Education Institutions. Use when client requests SDG reporting, NAAC Attributes 7/9/10 prep, NIRF SDG ranking data, THE Impact Rankings data, SDG audit, sustainability report. Do NOT use for general sustainability advice, ESG for corporates, environmental audits unrelated to SDGs, campus green audit without accreditation context."
argument-hint: [new-client|setup|audit|report|deliver|status|debug|verify|clients|analyze|case-study|alerts|cancel|archive|prep-machine]
metadata:
  author: JICATE Solutions
  version: 1.0.0
  category: service-delivery
---

# SDG Intelligence — Full Delivery Skill

You are delivering AI-powered SDG mapping and accreditation report generation for Higher Education Institutions for a client.

**User's command:** `$ARGUMENTS`

---

## ARCHITECTURE — HOW THIS SERVICE WORKS

```
[INPUT]                    [PROCESSING]                 [OUTPUT]
┌──────────────────┐      ┌──────────────────┐         ┌──────────────────┐
│ Institutional     │─────►│ Claude Code +     │────────►│ SDG Reports      │
│ data (CSV/Excel)  │      │ Claude AI +       │         │ (6 types)        │
│ courses, research │      │ Pandoc (PDF)      │         │ Markdown + PDF   │
│ outreach, ops     │      │                   │         │                  │
└──────────────────┘      └──────────────────┘         └──────────────────┘
```

**What the client gets:** SDG Scorecard, NAAC Attributes 7/9/10 sections, NIRF SDG data, THE Impact data, QS Sustainability data, Gap Analysis — all as ready-to-use PDF+Markdown.
**What we retain (MO):** Client data store, mapping history, quarterly schedule, gap evolution tracking, peer benchmark database.

### Service Type

**Type:** `standard` — all phases apply (ongoing delivery component for MO clients).

---

## WHAT THE OPERATOR NEEDS TO KNOW

The operator runs 4 commands total:

1. **`/sdg-intelligence new-client`** → registers the institution, creates client directory
2. **`/sdg-intelligence setup {client-id}`** → ingests CSV data, validates, normalizes
3. **`/sdg-intelligence audit {client-id}`** → AI maps all data to 17 SDGs, generates scorecard
4. **`/sdg-intelligence verify {client-id}`** → proves end-to-end delivery

That's it. The skill handles the rest.

> **First-time delivery flow:** `new-client` → `setup` → `audit` → `report {id} all` → `verify`. Or use `deliver` to run everything in sequence.

---

## COMMUNICATION RULES

**ALWAYS use plain language.** The operator is a business person, not a developer.

**Positioning:** JICATE delivers RESULTS, not tools. The client never touches AI. They receive finished reports.

| DO say | DON'T say |
|--------|-----------|
| "Here's your SDG scorecard" | "Generated JSON mapping output" |
| "Your NAAC sections are ready" | "Markdown report rendered" |
| "17 SDGs analyzed" | "Prompt chain executed" |
| "We found 12 strong SDGs and 5 gaps" | "Coverage percentage calculated" |
| "We handle everything securely" | "We use Claude Code for inference" |

When something goes wrong, explain:
1. **What happened** (one sentence)
2. **What to do** (specific action)
3. **Who to call** (if operator can't fix it — "contact the tech team")

---

## KEY TERMS

| Term | Meaning |
|------|---------|
| **PaP** | Pipeline as Product — one-time delivery. Client gets reports and manages themselves. |
| **MO** | Managed Operations — annual retainer. JICATE delivers quarterly updates. |
| **Gate** | A checkpoint that MUST pass before the next phase begins. If a gate fails, STOP. |
| **Operator** | The JICATE team member running this skill (not the end client). |
| **Client** | The HEI (Higher Education Institution) receiving the service. |
| **SDG Mapping** | The AI-generated link between an institutional data item and one or more of the 17 UN SDGs. |
| **Scorecard** | Summary of all 17 SDGs with coverage %, evidence count, and strength tier. |
| **Strength Tier** | Strong (>60%), Moderate (30-60%), Weak (10-30%), None (<10%) coverage. |

---

## SECURITY REQUIREMENTS

**Data sensitivity:** `low` — institutional academic data, no PII.

### Credential Handling
- No external API keys needed (Claude is built-in, Pandoc is local)
- Client data stored locally at `~/.claude/skills/sdg-intelligence/clients/{client-id}/`
- Never expose client institution names in conversation when discussing multi-client operations

### Data Handling
- Client data is confidential by default
- **Data retention:** Delete or archive client data within 90 days after service completion (PaP) or contract end (MO)
- **Data isolation:** Each client has its own directory. Never cross-reference data between clients without anonymization.
- **Governing law:** All agreements governed by laws of India, jurisdiction courts of Erode, Tamil Nadu

---

## COMMAND ROUTING

| Command | Action |
|---------|--------|
| `prep-machine` | PHASE -1 → Check Pandoc installed (optional) |
| `new-client` | PHASE 0 → Register institution → Create client directory |
| `setup {client-id}` | PHASE 1 → 2 → 3 → Ingest CSVs, validate, normalize |
| `audit {client-id}` | PHASE 4 → AI SDG mapping → Generate scorecard |
| `run {client-id}` | Alias for `audit` |
| `report {client-id} [type]` | Generate report: `naac`, `nirf`, `the`, `qs`, `scorecard`, `gap`, `all` |
| `deliver {client-id}` | Full sequence: setup → audit → report all → verify |
| `status {client-id}` | PHASE 5 → Health report |
| `debug {client-id}` | PHASE 6 → Diagnose issues |
| `verify {client-id}` | PHASE 7 → End-to-end proof |
| `clients` | Show all clients' status in one table |
| `analyze {client-id}` | Deep gap analysis + peer benchmarking + recommendations |
| `case-study {client-id}` | Auto-generate 1-page case study from delivery data |
| `alerts` | Check for stale reports, upcoming NAAC deadlines |
| `cancel {client-id}` | Graceful cancellation → archive → revoke access |
| `archive {client-id}` | Archive completed/cancelled client |
| _(empty/unclear)_ | Ask what they need in plain language |

---

## PHASE -1: ENVIRONMENT PREP (Optional)

### -1.1 Check prerequisites

Verify the operator's machine has:
- [ ] Claude Code (already running — this IS the environment)
- [ ] Pandoc for PDF generation (optional — `brew install pandoc`)
- [ ] Client data files in CSV format ready for ingestion

### -1.2 Install missing dependencies

```bash
# Only needed for PDF generation
brew install pandoc
```

### -1.3 Verify environment

**GATE: If Pandoc is missing, warn but continue — skill works without it (Markdown only, no PDF).**

> "Environment ready. Pandoc: [installed/not installed — PDF generation will be skipped]."

---

## PHASE 0: PROVISION CLIENT

### 0.1 Collect client information

Ask the operator:
- **Institution name?**
- **Delivery model: one-time (PaP) or managed service (MO)?**
- **How many academic departments does the institution have?**
- **Can they provide a CSV/Excel export of their course catalog?**
- **Does the institution have existing SDG tagging on any activities?**

Store delivery model = `pap` or `mo`.

### 0.2 Generate client ID

Generate a kebab-case slug from the institution name (e.g., "SRM Institute" → `srm-institute`).

**Collision check:** Verify `~/.claude/skills/sdg-intelligence/clients/{client-id}/` does not already exist.

### 0.3 Create client directory structure

```bash
mkdir -p ~/.claude/skills/sdg-intelligence/clients/{client-id}/{data,mappings,reports,history}
```

### 0.4 Write metadata file

Create `clients/{client-id}/metadata.json`:
```json
{
  "client_id": "{client-id}",
  "institution_name": "{name}",
  "delivery_model": "pap|mo",
  "departments": {count},
  "created_date": "{ISO date}",
  "operator": "{operator name}",
  "status": "active"
}
```

### 0.5 Show confirmation to operator

```
============================================
  CLIENT READY
============================================

  1. Client ID:     {client-id}
  2. Data dir:      ~/.claude/skills/sdg-intelligence/clients/{client-id}/data/
  3. Status:        Active

  NEXT STEPS:
  → Place CSV files in the data/ directory
  → Then run: /sdg-intelligence setup {client-id}
============================================
```

---

## PHASE 1: COLLECT SERVICE INPUTS

Ask the operator to place CSV files in `clients/{client-id}/data/`:
- **courses.csv** — course catalog with descriptions and learning outcomes
- **research.csv** — research publications with abstracts
- **outreach.csv** — outreach and extension activities
- **campus-ops.csv** — campus operations metrics (energy, water, waste)
- **initiatives.csv** — institutional initiatives (NSS, NCC, eco-clubs)
- **policies.csv** — institutional policies

Provide template CSVs from `references/templates/` if the institution needs help formatting.

Validate each file: check required columns exist, no empty critical fields, encoding is UTF-8.

**GATE: At least courses.csv OR research.csv must be present with >10 rows. Without this minimum data, SDG mapping is too sparse to be meaningful.**

---

## PHASE 2: CONFIGURE

### 2.0 Check known fixes (MANDATORY)

Before proceeding, read `references/known-fixes.md` and apply ALL listed fixes.

**GATE: If known-fixes.md has unapplied fixes relevant to this client's data type, apply them BEFORE proceeding.**

### 2.1 Normalize data

- Standardize department names (handle variations like "Dept. of CS" vs "Computer Science Department")
- Normalize date formats to ISO 8601
- Clean encoding issues (especially Tamil text in descriptions)

### 2.2 Generate data summary

Count rows per data type, identify any data quality issues.

### 2.3 Verify configuration

Tell the operator: "Data ingested: {X} courses, {Y} research papers, {Z} outreach activities, {W} campus ops metrics, {V} initiatives, {U} policies."

**GATE: If total data items < 20, warn: "Low data volume. Scorecard will be sparse. Consider adding more data sources."**

---

## PHASE 3: CONNECT & VALIDATE

### 3.1 Verify data files are readable

Read each CSV, confirm parsing succeeds, no corrupted rows.

### 3.2 Verify output directory is writable

Confirm `clients/{client-id}/mappings/` and `clients/{client-id}/reports/` are writable.

### 3.3 Validate data quality

- Check for duplicate entries
- Flag rows with empty descriptions (these can't be SDG-mapped)
- Warn about very short descriptions (<10 words) — lower mapping confidence

Tell the operator: "Data validated. {N} items ready for SDG mapping. {M} items flagged (empty/short descriptions)."

---

## PHASE 4: EXECUTE CORE PIPELINE

Tell the operator: "Now running SDG Intelligence mapping. This may take 10-30 minutes depending on data volume."

### 4.1 Ingest and Validate Data

Read all CSV files from client data directory. Parse and normalize. Create internal data structure for mapping.

### 4.2 AI SDG Mapping

For EACH data item (course, research paper, outreach activity, initiative, policy):

**Prompt pattern:**
```
Analyze this {type} from a Higher Education Institution and map it to relevant UN Sustainable Development Goals.

{Item data: name, description, outcomes, etc.}

For each relevant SDG, provide:
1. SDG number (1-17)
2. Specific target(s) (e.g., 4.1, 13.3)
3. Confidence score (0-100)
4. Brief reasoning (1 sentence)

Only map to SDGs where there is genuine alignment. Do not force-map.
Confidence guide: 90+ = explicit mention, 70-89 = strong inference, 50-69 = moderate inference, <50 = weak/tangential.

FEW-SHOT EXAMPLES:

Example 1 — Course: "Community Health Nursing" (Nursing dept)
Description: "Community-based healthcare delivery focusing on maternal and child health, nutrition, sanitation, communicable disease prevention, and health education for rural and tribal populations."
→ {"mappings": [
  {"sdg": 3, "targets": ["3.1","3.2","3.3"], "confidence": 95, "reasoning": "Directly addresses maternal health, child health, and communicable disease prevention"},
  {"sdg": 2, "targets": ["2.1","2.2"], "confidence": 75, "reasoning": "Nutrition component addresses food security and malnutrition"},
  {"sdg": 6, "targets": ["6.2"], "confidence": 70, "reasoning": "Sanitation education contributes to clean water and sanitation goals"},
  {"sdg": 10, "targets": ["10.2"], "confidence": 65, "reasoning": "Focus on rural and tribal populations addresses reducing inequalities"},
  {"sdg": 4, "targets": ["4.3"], "confidence": 60, "reasoning": "Health education component serves as inclusive quality education"}
]}

Example 2 — Course: "Renewable Energy Systems" (Engineering dept)
Description: "Design and analysis of solar PV, wind energy, biomass, and hybrid renewable energy systems. Focus on off-grid solutions for rural electrification."
→ {"mappings": [
  {"sdg": 7, "targets": ["7.1","7.2"], "confidence": 95, "reasoning": "Directly covers affordable and clean energy including solar, wind, biomass"},
  {"sdg": 13, "targets": ["13.3"], "confidence": 85, "reasoning": "Renewable energy directly contributes to climate action"},
  {"sdg": 9, "targets": ["9.4"], "confidence": 70, "reasoning": "Sustainable infrastructure through clean technology"},
  {"sdg": 11, "targets": ["11.6"], "confidence": 60, "reasoning": "Rural electrification contributes to sustainable communities"}
]}

Example 3 — Research: "Biodegradable packaging from agricultural waste"
Abstract: "Development of eco-friendly biodegradable packaging materials from rice husk and sugarcane bagasse as alternatives to plastic."
→ {"mappings": [
  {"sdg": 12, "targets": ["12.5"], "confidence": 92, "reasoning": "Directly converts waste to useful product — responsible consumption and production"},
  {"sdg": 9, "targets": ["9.4","9.5"], "confidence": 80, "reasoning": "Industrial innovation with sustainable manufacturing"},
  {"sdg": 13, "targets": ["13.3"], "confidence": 70, "reasoning": "Reducing plastic use contributes to climate action"},
  {"sdg": 15, "targets": ["15.1"], "confidence": 55, "reasoning": "Reducing plastic pollution protects terrestrial ecosystems"}
]}

Now analyze the provided item:

Respond in JSON format:
{"mappings": [{"sdg": N, "targets": ["N.M"], "confidence": NN, "reasoning": "..."}]}
```

**For campus operations:** Use rule-based mapping from `references/campus-ops-rules.md` first, then AI for edge cases:
- Energy metrics → SDG 7 (Affordable and Clean Energy)
- Water metrics → SDG 6 (Clean Water and Sanitation)
- Waste metrics → SDG 12 (Responsible Consumption and Production)
- Transport/emissions → SDG 13 (Climate Action)
- Green cover → SDG 15 (Life on Land)

Save all mappings to `clients/{client-id}/mappings/sdg-mappings.json`.

### 4.3 Aggregate and Score

- Count evidence items per SDG (1-17)
- Calculate coverage % = (items mapped to this SDG / total items) * 100
- Assign strength tier:
  - **Strong:** >60% coverage
  - **Moderate:** 30-60% coverage
  - **Weak:** 10-30% coverage
  - **None:** <10% coverage
- Generate `clients/{client-id}/mappings/scorecard.json`

### 4.4 Verify execution

Tell the operator: "SDG mapping complete. {N} items mapped across {M}/17 SDGs. Average confidence: {avg}%." or "Partial mapping — {N} items failed (empty descriptions). {M} items mapped successfully."

---

## PHASE 5: HEALTH REPORT

```
============================================
  SDG Intelligence — HEALTH REPORT
============================================
  Client:            {client_name}
  Service:           standard
  Status:            OK / WARNING / ERROR

  Data:              {N} items ingested
  Mappings:          {M} items mapped
  Reports:           {R} generated

  Last audit:        {timestamp}
  Next scheduled:    {timestamp} (MO only)

  Key metrics:
  - Total items mapped:     {count}
  - SDGs covered (>10%):    {count}/17
  - Avg confidence score:   {avg}%
============================================
```

---

## PHASE 6: DEBUG & DIAGNOSE

Run checks and explain in plain language:

1. **Are data files present?** Check `clients/{id}/data/` for CSV files.
2. **Are data files valid?** Try parsing each CSV.
3. **Do mappings exist?** Check `clients/{id}/mappings/sdg-mappings.json`.
4. **Are reports generated?** Check `clients/{id}/reports/` for .md files.

Plain-language decision tree:
```
Problem: No data files found
Fix: Operator must place CSV files in clients/{id}/data/ directory. Use template CSVs from references/templates/

Problem: AI mapping returned empty results
Fix: Check if course descriptions are empty. AI needs text to analyze. Minimum 10 words per description.

Problem: Report generation failed
Fix: Check if mappings exist (run audit first). Reports need mapping data. Run: /sdg-intelligence audit {id}

Problem: Low confidence scores (<50% average)
Fix: Descriptions may be too short or in non-English. Ask institution for detailed English descriptions.

Problem: Can't figure it out.
Fix: Contact the tech team with: client name, what step failed, error message.
```

---

## PHASE 7: END-TO-END VERIFICATION

### 7.1 Pipeline check

| Step | Check | Expected |
|------|-------|----------|
| Input accessible | Data CSV files readable | All CSV files present with >0 rows |
| Processing ran | sdg-mappings.json exists | File exists with >0 mapping entries |
| Output delivered | Report .md files exist | All requested report types in reports/ |
| Quality check | Reports have content | Each report >500 words, no empty sections |

### 7.2 Delivery report

```
============================================
  DELIVERY COMPLETE
============================================
  Client:     {client_name}
  Service:    SDG Intelligence

  Pipeline:   CSV Data → AI Mapping → Reports
  Status:     ALL CHECKS PASSED

  Delivered:
  - SDG Scorecard: 17 SDGs scored with strength tiers
  - NAAC Attributes 7/9/10: Ready for SSR paste
  - NIRF SDG Rankings: Parameter evidence sheets
  - THE Impact Rankings: Per-SDG evidence
  - QS Sustainability: 3-dimension data
  - Gap Analysis: Prioritized recommendations

  Client ID:     {id}
  Reports at:    clients/{id}/reports/
============================================
```

### 7.3 Handover — PaP Mode (one-time delivery)

1. Present delivery report
2. Share all report files with client (PDF + Markdown via WhatsApp/email)
3. Inform client: 30-day support included
4. Operator completes delivery certificate
5. **Service complete** — client self-manages

> "Delivery complete. Client has all 6 reports. 30-day support period active."

### 7.4 Handover — MO Mode (managed operations)

1. Present delivery report
2. Share reports via client's preferred channel
3. Configure quarterly update schedule (next audit in 3 months)
4. Set up NAAC/NIRF deadline alerts
5. **Ongoing service active**

> "Delivery complete. Quarterly updates scheduled. Next audit: {date}."

MO ongoing responsibilities:
- Quarterly re-audit with fresh data
- Monitor NAAC/NIRF submission deadlines
- Send updated reports before deadlines
- Run `/sdg-intelligence analyze` for gap evolution tracking

---

## OPERATOR QUICK REFERENCE

### Delivery commands
| Command | What it does |
|---------|-------------|
| `/sdg-intelligence prep-machine` | Check/install Pandoc (first-time only) |
| `/sdg-intelligence new-client` | Register new institution |
| `/sdg-intelligence setup {id}` | Ingest and validate CSV data |
| `/sdg-intelligence audit {id}` | AI maps all data to SDGs |
| `/sdg-intelligence report {id} [type]` | Generate specific report type |
| `/sdg-intelligence deliver {id}` | Full first-time: setup → audit → report all → verify |
| `/sdg-intelligence status {id}` | Check if everything works |
| `/sdg-intelligence debug {id}` | Find and fix problems |
| `/sdg-intelligence verify {id}` | Prove end-to-end delivery |

### Management commands
| Command | What it does |
|---------|-------------|
| `/sdg-intelligence clients` | All clients' status in one table |
| `/sdg-intelligence alerts` | Check for stale reports, upcoming deadlines |
| `/sdg-intelligence analyze {id}` | Gap analysis + peer benchmarking |
| `/sdg-intelligence case-study {id}` | Auto-generate case study |
| `/sdg-intelligence cancel {id}` | Cancel service, archive data |
| `/sdg-intelligence archive {id}` | Archive completed client |

---

## `/sdg-intelligence analyze {client-id}` — Value-Add Report

**Purpose:** Generate deep insights from mapping data (upsell opportunity)

1. Read client's scorecard and mappings
2. Generate analysis:
   - **SDG Coverage Trend** — how coverage has changed since last audit (MO clients)
   - **Peer Benchmarking** — anonymous comparison with other JICATE clients
   - **Quick Wins** — 5 specific actions to improve weakest SDGs with minimal effort
3. Save to vault: `Capture/JICATE-Hub/sdg-intelligence-analysis-{client-id}-{date}.md`
4. Display summary with key highlights

---

## `/sdg-intelligence case-study {client-id}` — Auto-Generate Case Study

**Purpose:** Generate 1-page case study for marketing/sales.

```markdown
# Case Study: {client_name}

## Client Profile
- **Industry:** Higher Education
- **Size:** {departments} departments, {courses} courses

## Challenge
{client_name} needed SDG reporting and accreditation data preparation for NAAC, NIRF, and international rankings.

## Solution
JICATE Solutions deployed SDG Intelligence — AI-powered auto-mapping of institutional data to 17 UN SDGs with accreditation-ready report generation.

## Results
- **SDGs Mapped:** {count}/17 with evidence
- **Items Analyzed:** {total_items} courses, research, outreach activities
- **Average Confidence:** {avg}%
- **Reports Delivered:** 6 (Scorecard, NAAC, NIRF, THE, QS, Gap Analysis)

## Client Testimonial
> "[Placeholder — request from client]"
```

Save to vault and tell operator to customize before sharing.

---

## `/sdg-intelligence clients` — Multi-Client Monitor

```
Institution          Last Audit             Status     Model   SDGs Covered
-----------          ----------             ------     -----   ------------
JKKN Engineering     2026-03-22 14:32       OK         MO      14/17
SRM Chennai          2026-03-20 09:15       OK         PaP     16/17
Nehru Arts           2026-02-28 11:00       STALE      MO      11/17
```

**Status logic:**
- **OK** = last audit within expected interval (30 days PaP, 90 days MO)
- **STALE** = overdue (check with client, re-audit needed)
- **ERROR** = last audit had errors (investigate with debug)

---

## CANCELLATION & FAILURE PROTOCOL

### `/sdg-intelligence cancel {client-id}`

1. Confirm with operator: "This will stop all service for {client-name}. Proceed? (yes/no)"
2. Export client data to archive: `Capture/JICATE-Hub/sdg-intelligence-archive-{client-id}/`
3. Remove from active client directory
4. Tell operator: "Client cancelled. Data archived locally."

### Delivery Failure Handling

If any phase fails irrecoverably:
1. **Document** what failed and why (add to known-fixes.md)
2. **Notify** operator with plain-language explanation
3. **Preserve** all client data and partial work
4. **Options:** retry from last gate, add more data, or escalate

---

## KNOWN EDGE CASES & KEY FACTS

### Service-specific
- Institutions with <50 courses produce sparse scorecards — warn operator and suggest supplementing with research/outreach data
- Tamil-medium course descriptions may produce lower-confidence SDG mappings — AI works best with English descriptions
- Some institutions have pre-tagged SDG data (sdg_self_tagged column) — use this as a validation signal, not as the mapping itself

### Data quality
- Empty descriptions = unmappable items. Flag and exclude from coverage calculations.
- Very short descriptions (<10 words) get confidence penalty of -20 points
- Duplicate courses across semesters should be deduplicated before mapping

### Operator environment
- Pandoc not installed — skill works without it (Markdown only, no PDF). Install with `brew install pandoc`

---

## EXAMPLES

**First-time delivery:**
```
User: /sdg-intelligence new-client
Skill: "What's the institution name?" → "Delivery model: PaP or MO?" → creates client directory
Skill: "Client ready. ID: srm-chennai. Place CSVs in data/ directory, then run: /sdg-intelligence deliver srm-chennai"

User: /sdg-intelligence deliver srm-chennai
Skill: validates data → maps to SDGs → generates all 6 reports → shows delivery report
```

**Generate specific report:**
```
User: /sdg-intelligence report srm-chennai naac
Skill: generates NAAC Attributes 7/9/10 sections → saves to reports/ → shows summary
```

**Check on all clients:**
```
User: /sdg-intelligence clients
Skill: shows table with all institutions, last audit date, status, SDG coverage
```

---

## REFERENCE FILES

See the `references/` directory:
- [SDG Reference](references/sdg-reference.md) — All 17 SDGs + 169 targets
- [NAAC Mapping](references/naac-attributes-7-9-10-mapping.md) — NEW binary framework Attributes 7, 9, 10
- [NIRF SDG Parameters](references/nirf-sdg-parameters.md) — NEW 2025 SDG ranking category
- [THE Impact Indicators](references/the-impact-indicators.md) — Per-SDG evaluation methodology
- [QS Sustainability](references/qs-sustainability-dimensions.md) — 3-dimension framework
- [Campus Ops Rules](references/campus-ops-rules.md) — Rule-based SDG mapping for operations
- [Operator Playbook](references/operator-playbook.md) — Printable step-by-step checklist
- [SOW](references/sow.md) — Statement of work for client signature
- [Delivery Certificate](references/delivery-certificate.md) — Sign-off document
- [Quotation](references/quotation.md) — Pricing for PaP/MO
- [Known Fixes](references/known-fixes.md) — Issues found and fixed across deployments
- [Onboarding Form](references/onboarding-form.md) — Client intake questionnaire
- [CSV Templates](references/templates/) — Column templates for all 6 data types
- [Sample Data](references/templates/sample-data/) — JKKN test data for validation
