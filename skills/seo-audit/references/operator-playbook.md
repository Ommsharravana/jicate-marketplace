# SEO+GEO+AEO Audit — Operator Playbook

## Client Delivery Checklist

Print this. Follow it step by step. Tick each box.

---

### BEFORE YOU START

- [ ] **1. Open Claude Code on your computer**
- [ ] **2. Type:** `/seo-audit new-client`
- [ ] **3. Answer the questions** (client name, URLs, tier, competitors, queries, delivery model)
- [ ] **4. Wait** — Claude provisions the client (1-2 minutes)
- [ ] **5. Save the values shown on screen:**

```
  Client slug:   ___________________________
  URLs:          ___________________________
  Tier:          ___________________________
  Model:         ___________________________
```

---

### RUN THE AUDIT

- [ ] **6. Type:** `/seo-audit run --client {slug}`
- [ ] **7. Wait for the pipeline to finish** — Claude shows progress
  - Single site (Basic): ~30-60 minutes
  - Single site (Premium): ~60-90 minutes
  - Multi-site (10 URLs): ~3-4 hours
- [ ] **8. Review the report** — Claude shows the summary with scores

---

### GENERATE PDF AND DELIVER

- [ ] **9. Type:** `/seo-audit deliver --client {slug}`
- [ ] **10. Claude generates PDF and shows the file path**
- [ ] **11. Send PDF to client via WhatsApp or email** (do this yourself — Claude won't auto-send)
- [ ] **12. Explain what they're getting:**
  - "This is your complete digital health report — SEO, AI visibility, and competitor comparison"
  - "The action plan is prioritized — start with Priority 1 items this week"
  - "We can walk through the findings together if you'd like"

---

### VERIFY (don't skip this!)

- [ ] **13. Type:** `/seo-audit verify --client {slug}`
- [ ] **14. Check the delivery report:**
  - [ ] All pipeline steps say PASSED
  - [ ] Report has all sections filled
  - [ ] PDF opens correctly
  - [ ] Scores look reasonable (not all zeros, not all 100s)

---

### SAVE A RECORD

- [ ] **15. Save a record:**

```
  Client:         ___________________________
  Date:           ___________________________
  Client slug:    ___________________________
  Score:          _____ / 100
  Contact Person: ___________________________
  Phone:          ___________________________
  Delivery Model: PaP / MO (circle one)
  Next check-in:  ___________________________ (MO only)
```

---

## TROUBLESHOOTING

### "Firecrawl credits exhausted"

1. Check remaining credits: operator asks Claude
2. If out: wait for monthly reset, or upgrade Firecrawl plan
3. Still stuck? Contact the tech team

### "AI model API not responding"

1. Claude will try alternate models automatically
2. If all 3 models are down: you'll get an SEO-only report with a note
3. Re-run GEO/AEO later: `/seo-audit run --client {slug}`

### "GSC token expired" (Premium clients only)

1. Have the client re-authorize access
2. Claude will walk you through the process
3. If client can't authorize: downgrade to Basic tier for now

### Nothing works?

Call the tech team. Have ready:
- Client name and slug
- What step you were on
- Screenshot of the error

---

## QUICK COMMANDS

| What you want to do | What to type |
|---------------------|-------------|
| Check machine setup | `/seo-audit prep-machine` |
| Set up new client | `/seo-audit new-client` |
| Run the audit | `/seo-audit run --client {slug}` |
| Generate PDF | `/seo-audit deliver --client {slug}` |
| Check status | `/seo-audit status --client {slug}` |
| Fix problems | `/seo-audit debug --client {slug}` |
| Full verification | `/seo-audit verify --client {slug}` |
| See all clients | `/seo-audit clients` |
| Cancel service | `/seo-audit cancel --client {slug}` |
| Delete all data | `/seo-audit delete-client --client {slug}` |
| Revoke share link | `/seo-audit revoke-link --client {slug}` |

---

## TIMINGS

| What | How long |
|------|----------|
| Client provisioning | 1-2 minutes |
| Single site Basic audit | 30-60 minutes |
| Single site Premium audit | 60-90 minutes |
| Multi-site audit (10 URLs) | 3-4 hours |
| PDF generation | 2-5 minutes |

---

## TECH SUPPORT

```
Name:     S Ommsharravana
Email:    director@jkkn.ac.in
Hours:    Mon-Sat, 9 AM - 6 PM IST
```
