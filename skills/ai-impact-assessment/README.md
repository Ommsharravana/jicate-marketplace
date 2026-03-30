# AI Impact Assessment

**Find THE ONE THING that changes everything for any business.**

Give it a company name and city. It auto-researches, analyzes, and generates a personalized AI impact report that makes business owners pick up the phone.

## What It Does

```
Input:  "GLCS Salem" (company name + city)
           ↓
Auto:   WebSearch + Firecrawl + Directories + Industry Research
           ↓
Find:   THE ONE THING — the single irresistible AI opportunity
           ↓
Output: Personalized report with real numbers, proof, and one clear next step
```

## The Irresistibility Framework

The skill doesn't produce a generic list of 10 opportunities. It finds the ONE thing that makes saying "no" feel irrational:

1. **Revenue Bottleneck** — what constraint limits their money-making?
2. **Cost of Nothing** — put a painful number on it
3. **The Flip** — show found money, not a cost
4. **Tangible** — explain it so a 10-year-old gets it
5. **Proof** — real case study of a similar business
6. **Irresistible CTA** — "see it on your data before you pay"

## Quick Start

```bash
# Install
cp -r ai-impact-assessment ~/.claude/skills/

# In Claude Code:
/ai-impact-assessment new-client      # Just give company name + city
/ai-impact-assessment run {id}        # Auto-research + analysis + report
/ai-impact-assessment deliver {id}    # Run + verify in one go
```

## Minimum Input Required

| What you provide | What the skill finds automatically |
|-----------------|-----------------------------------|
| Company name | Website, industry, services, team size |
| City | Address, competitors, market context |
| Contact name (optional) | Phone, email, LinkedIn from website |

Everything else — industry trends, pain points, AI opportunities, ROI estimates — is auto-researched and inferred.

## Commands

| Command | What it does |
|---------|-------------|
| `new-client` | Register business (auto-researches immediately) |
| `run {id}` | Deep research + analysis + report generation |
| `deliver {id}` | Run + verify in one go |
| `verify {id}` | Quality check the report |
| `clients` | See all assessments and their status |
| `case-study {id}` | Auto-generate a case study |

## Files

```
ai-impact-assessment/
├── SKILL.md                    # Main skill (v2.0.0, 3507 words)
└── references/
    ├── operator-playbook.md    # Printable delivery checklist
    ├── onboarding-form.md      # Client intake with discovery questions
    ├── quotation.md            # FREE assessment + follow-up pricing
    ├── sow.md                  # Statement of work
    ├── delivery-certificate.md # Sign-off document
    └── known-fixes.md          # Issues found in real deployments
```

## Version History

| Version | What Changed |
|---------|-------------|
| 2.0.0 | "The One Thing" — irresistibility framework replaces generic opportunity lists |
| 1.3.0 | Inferred pain points with skip-to-analysis option |
| 1.2.0 | Auto-research moves to registration (Phase 0) |
| 1.1.0 | Auto-research added (WebSearch + Firecrawl before asking questions) |
| 1.0.0 | Initial skill with manual discovery interview |

---

**Part of [JICATE Marketplace](https://github.com/Ommsharravana/jicate-marketplace) — AI services by JICATE Solutions**
