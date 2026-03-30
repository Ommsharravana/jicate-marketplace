# SDG Intelligence — Operator Playbook

## Client Delivery Checklist

Print this. Follow it step by step. Tick each box.

---

### BEFORE YOU START

- [ ] **1. Open Claude Code on your computer**
- [ ] **2. Type:** `/sdg-intelligence new-client`
- [ ] **3. Answer the questions** (institution name, AISHE code, delivery model, IQAC contact)
- [ ] **4. Wait** — Claude provisions the client directory (1-2 minutes)
- [ ] **5. Save the values shown on screen:**

```
  Client ID:     ___________________________
  Data Directory: ~/.claude/skills/sdg-intelligence/clients/{id}/
  Contact Person: ___________________________
```

---

### COLLECT DATA FROM INSTITUTION

- [ ] **6. Send the CSV templates to the IQAC Coordinator:**
  - `references/templates/courses.csv` — Course catalog with descriptions
  - `references/templates/research.csv` — Research publications
  - `references/templates/outreach.csv` — Outreach/extension activities
  - `references/templates/campus-ops.csv` — Campus operations metrics (energy, water, waste)
  - `references/templates/initiatives.csv` — NSS/NCC/eco-club and other initiatives
  - `references/templates/policies.csv` — Institutional policies
- [ ] **7. Send the sample data files** as examples of how to fill them:
  - `references/templates/sample-data/jkkn-courses-sample.csv`
  - `references/templates/sample-data/jkkn-research-sample.csv`
  - `references/templates/sample-data/jkkn-outreach-sample.csv`
- [ ] **8. Set a deadline** — Tell the institution: "We need all CSV files within 7 working days."
- [ ] **9. When files arrive**, place them in `clients/{client-id}/data/`

> **Minimum data threshold:** At least 50 courses + some research OR outreach data. Below this, the scorecard will be too sparse to be useful. Push back if data is insufficient.

---

### SET UP THE CLIENT

- [ ] **10. Type:** `/sdg-intelligence setup {client-id}`
- [ ] **11. Claude validates all CSV files** — fix any errors it reports:
  - Missing required columns
  - Empty description fields (AI needs text to analyze)
  - Encoding issues (common with Tamil text)
- [ ] **12. Wait for "Data ingested successfully"** confirmation

---

### RUN THE SDG AUDIT

- [ ] **13. Type:** `/sdg-intelligence audit {client-id}`
- [ ] **14. Wait for the AI mapping to finish:**
  - Small institution (50-200 items): 5-10 minutes
  - Medium institution (200-500 items): 10-20 minutes
  - Large institution (500+ items): 20-30 minutes
- [ ] **15. Review the SDG scorecard output** — sanity check the top 3 SDGs. Do they make sense for this institution?

> **Shortcut:** Use `/sdg-intelligence deliver {client-id}` to run setup, audit, report all, and verify in one go.

---

### GENERATE REPORTS

- [ ] **16. Type:** `/sdg-intelligence report {client-id} all`
  - This generates all 6 report types: scorecard, naac, nirf, the, qs, gap
- [ ] **17. Or generate specific reports:**
  - `/sdg-intelligence report {client-id} scorecard` — SDG Scorecard (visual heat map)
  - `/sdg-intelligence report {client-id} naac` — NAAC Attributes 7, 9 & 10 sections
  - `/sdg-intelligence report {client-id} nirf` — NIRF SDG Rankings data
  - `/sdg-intelligence report {client-id} the` — THE Impact Rankings data
  - `/sdg-intelligence report {client-id} qs` — QS Sustainability data
  - `/sdg-intelligence report {client-id} gap` — Gap Analysis with recommendations

---

### VERIFY (don't skip this!)

- [ ] **18. Type:** `/sdg-intelligence verify {client-id}`
- [ ] **19. Check the verification report:**
  - [ ] All 6 CSV data types have >0 rows
  - [ ] SDG mappings JSON exists with entries for all data items
  - [ ] All requested report .md files exist in reports/
  - [ ] Each report is >500 words with no empty sections
  - [ ] At least 8 of 17 SDGs have some coverage

---

### HANDOVER TO CLIENT

- [ ] **20. Share the reports with the IQAC Coordinator:**
  - "Here is your institution's SDG Scorecard showing alignment across all 17 Sustainable Development Goals."
  - "The NAAC sections for Attributes 7, 9, and 10 are ready to paste into your SSR document."
  - "We have also prepared data sheets for NIRF SDG, THE Impact, and QS Sustainability rankings if you plan to participate."

- [ ] **21. Walk through the Gap Analysis** — explain the top 3 recommendations

- [ ] **22. Get the Delivery Certificate signed** (use `references/delivery-certificate.md`)

- [ ] **23. Save a record:**

```
  Client:         ___________________________
  Date:           ___________________________
  Client ID:      ___________________________
  IQAC Contact:   ___________________________
  Phone:          ___________________________
  Delivery Model: PaP / MO (circle one)
  Reports Delivered: scorecard / naac / nirf / the / qs / gap
  Next check-in:  ___________________________ (MO only — quarterly)
```

---

## TROUBLESHOOTING

### "No data files found"

1. Check that CSV files are placed in `clients/{client-id}/data/` directory
2. Verify filenames match expected names: courses.csv, research.csv, outreach.csv, campus-ops.csv, initiatives.csv, policies.csv
3. Still broken? Run `/sdg-intelligence debug {client-id}`

### "AI mapping returned empty results"

1. Check if course/research descriptions are empty — AI needs text content to analyze
2. Open the CSV and verify the description column has actual content, not just course codes
3. Still broken? Call the tech team.

### "Report generation failed"

1. Ensure the audit step was completed first — reports need mapping data
2. Run `/sdg-intelligence status {client-id}` to check if mappings exist
3. Re-run `/sdg-intelligence audit {client-id}` if mappings are missing

### Nothing works?

Call the tech team. Have ready:
- Client name and ID
- What step you were on
- Screenshot of the error

---

## QUICK COMMANDS

| What you want to do | What to type |
|---------------------|-------------|
| Set up new client | `/sdg-intelligence new-client` |
| Ingest data | `/sdg-intelligence setup {id}` |
| Run SDG mapping | `/sdg-intelligence audit {id}` |
| Generate reports | `/sdg-intelligence report {id} [type]` |
| Full first delivery | `/sdg-intelligence deliver {id}` |
| Check status | `/sdg-intelligence status {id}` |
| Fix problems | `/sdg-intelligence debug {id}` |
| Full verification | `/sdg-intelligence verify {id}` |
| Deep gap analysis | `/sdg-intelligence analyze {id}` |
| See all clients | `/sdg-intelligence clients` |
| Auto case study | `/sdg-intelligence case-study {id}` |
| Check all alerts | `/sdg-intelligence alerts` |

---

## TIMINGS

| What | How long |
|------|----------|
| Client provisioning | 1-2 minutes |
| Data ingestion (setup) | 2-5 minutes |
| SDG audit (small: 50-200 items) | 5-10 minutes |
| SDG audit (medium: 200-500 items) | 10-20 minutes |
| SDG audit (large: 500+ items) | 20-30 minutes |

---

## MANAGED OPERATIONS (MO) SCHEDULE

For MO clients, follow this quarterly cycle:

| Quarter | Action |
|---------|--------|
| Q1 (Apr-Jun) | Collect updated data, refresh all reports, deliver gap analysis |
| Q2 (Jul-Sep) | Mid-year scorecard update, NAAC prep if cycle year |
| Q3 (Oct-Dec) | THE Impact deadline prep, NIRF SDG data refresh |
| Q4 (Jan-Mar) | Annual review, QS Sustainability prep, year-over-year comparison |

---

## TECH SUPPORT

```
Name:     JICATE Tech Team
Phone:    [As assigned]
Hours:    Mon-Sat, 9 AM - 6 PM IST
```
