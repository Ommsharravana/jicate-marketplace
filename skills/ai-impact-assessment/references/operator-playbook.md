# AI Impact Assessment — Operator Playbook

## Client Delivery Checklist

Print this. Follow it step by step. Tick each box.

---

### BEFORE YOU START

- [ ] **1. Open Claude Code on your computer**
- [ ] **2. Type:** `/ai-impact-assessment new-client`
- [ ] **3. Answer the questions** (business name, industry, contact, source, pain point)
- [ ] **4. Wait** — Claude registers the business (under 1 minute)
- [ ] **5. Save the client ID shown on screen:**

```
  Client ID:     ___________________________
  Business:      ___________________________
  Industry:      ___________________________
```

---

### RUN THE ASSESSMENT

- [ ] **6. Type:** `/ai-impact-assessment run {client-id}`
- [ ] **7. Answer discovery questions in 5 batches**
   - Have the client on call or in front of you
   - If you don't know an answer, say "skip" and come back to it
- [ ] **8. Wait for the report to generate** (2-5 minutes after all questions answered)

> **Shortcut:** Use `/ai-impact-assessment deliver {client-id}` to run + verify in one go.

---

### VERIFY (don't skip this!)

- [ ] **9. Type:** `/ai-impact-assessment verify {client-id}`
- [ ] **10. Check the report:**
  - [ ] At least 3 specific opportunities (not generic)
  - [ ] Each one mentions something from THIS business
  - [ ] Savings estimates make sense
  - [ ] No technical jargon — a business owner can understand it

---

### SEND TO CLIENT

- [ ] **11. Send the report via WhatsApp (PDF) or Email**
- [ ] **12. Include this message:**
  - "Here's your personalized AI Impact Assessment. We found {N} specific opportunities where AI could save your business time and money."
  - "Would you like a 30-minute call to walk through the findings?"
  - "This assessment is free — no obligation."

- [ ] **13. Schedule walkthrough call** (within 3 days of sending report)

- [ ] **14. Save a record:**

```
  Business:       ___________________________
  Date Sent:      ___________________________
  Client ID:      ___________________________
  Contact Person: ___________________________
  Phone:          ___________________________
  Walkthrough:    Scheduled / Pending / Declined
  Conversion:     Pending
```

---

## DURING THE WALKTHROUGH CALL

**Talking points:**
1. "We analyzed YOUR specific business — this isn't a template"
2. Walk through each opportunity, starting with the highest impact
3. "Here's what it would look like in practice..." (use relatable examples)
4. "The Quick Wins can be done in 2-4 weeks for under ₹1L"
5. "Would you like to start with one of these?"

**If they're interested:**
- Offer to scope the top opportunity into a proposal
- Mention the partner discount if applicable (Yi, JKKN alumni, etc.)
- "We can have a prototype ready in 24-48 hours"

**If they need time:**
- "Take your time. The report is yours to keep."
- Schedule a follow-up in 2 weeks
- Add them to the follow-up tracker

---

## TROUBLESHOOTING

### "Client doesn't know how to answer the questions"

1. Rephrase in simpler terms — "What takes the most time in your day?"
2. Use examples — "For example, one textile company spent 3 hours daily on order tracking"
3. Skip and come back — partial info is better than no info

### "Report looks too generic"

1. Re-read the discovery answers — did you capture enough detail?
2. Run `/ai-impact-assessment run {client-id}` again with more specific answers
3. Add context: "This client specifically mentioned {detail}"

### Nothing works?

Call the tech team. Have ready:
- Client name and ID
- What step you were on
- Screenshot of the error

---

## QUICK COMMANDS

| What you want to do | What to type |
|---------------------|-------------|
| Register new business | `/ai-impact-assessment new-client` |
| Run assessment | `/ai-impact-assessment run {id}` |
| Full delivery | `/ai-impact-assessment deliver {id}` |
| Verify report | `/ai-impact-assessment verify {id}` |
| See all businesses | `/ai-impact-assessment clients` |
| Make a case study | `/ai-impact-assessment case-study {id}` |
| Cancel assessment | `/ai-impact-assessment cancel {id}` |
| Archive completed | `/ai-impact-assessment archive {id}` |

---

## TIMINGS

| What | How long |
|------|----------|
| Registration | Under 1 minute |
| Discovery interview | 15-20 minutes (depends on client) |
| Report generation | 2-5 minutes |
| Total delivery | 20-30 minutes |

---

## TECH SUPPORT

```
Name:     AI Engineering Team
Phone:    [Internal extension]
Hours:    Mon-Sat, 9am-6pm IST
```
