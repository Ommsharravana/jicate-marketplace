# JICATE Marketplace

**AI-powered service delivery skills for businesses.**

JICATE Solutions builds AI services that deliver results — not tools. Every skill in this marketplace is a battle-tested, repeatable service that turns a business name into a personalized deliverable.

## Available Skills

| Skill | What It Does | Input | Output | Cost |
|-------|-------------|-------|--------|------|
| [AI Impact Assessment](skills/ai-impact-assessment/) | Personalized AI opportunity analysis for any business | Company name + city | "The One Thing" report with ROI estimates | **Free** |

## How It Works

Each skill follows the **Pipeline Compound Loop** — more clients make the skill smarter, which makes delivery faster, which increases margin:

```
[Business Name] ──> [Auto-Research] ──> [AI Analysis] ──> [Personalized Report]
      │                    │                   │                    │
      │              WebSearch +          JICATE 5-Line         "The One Thing"
      │              Firecrawl +          Framework +           that changes
      │              Directories          Industry Intel        everything
      │                    │                   │                    │
      └────────────────────┴───────────────────┴────────────────────┘
                    Runs on Claude Code (Opus)
```

## For Business Owners

**Want a personalized look at how AI could impact YOUR specific business?**

Not generic advice. Not a template. A specific analysis of YOUR industry, YOUR workflows, YOUR problems — with real numbers showing what you're leaving on the table.

**What you get:**
- "The One Thing" — the single highest-impact AI opportunity for your specific business
- Real numbers — cost of doing nothing, savings when solved
- Proof — case studies from similar businesses
- Next step — see it working on your data, free, in 48 hours

**What it costs:** Nothing. The assessment is free.

**How to get one:** Send your company name to [JICATE Solutions](https://wa.me/917094453636?text=I%20want%20a%20free%20AI%20Impact%20Assessment%20for%20my%20business).

## For Claude Code Users

These skills run on [Claude Code](https://claude.ai/claude-code). Install any skill by copying it to your `~/.claude/skills/` directory.

```bash
# Install a skill
git clone https://github.com/Ommsharravana/jicate-marketplace.git
cp -r jicate-marketplace/skills/ai-impact-assessment ~/.claude/skills/

# Run it
# In Claude Code:
/ai-impact-assessment new-client
```

### Skill Architecture

Every skill follows the [Service Skill Template](https://github.com/Ommsharravana/jicate-marketplace/tree/main/skills/) pattern:

```
skills/{skill-name}/
├── SKILL.md                    # Main skill instructions
└── references/
    ├── operator-playbook.md    # Step-by-step for operators
    ├── onboarding-form.md      # Client intake
    ├── quotation.md            # Pricing templates
    ├── sow.md                  # Statement of work
    ├── delivery-certificate.md # Sign-off document
    └── known-fixes.md          # Issues found in real deployments
```

## About JICATE Solutions

JICATE Solutions is the AI services arm of [JKKN Educational Institutions](https://jkkn.ac.in) — India's First Human-AI Collaboration Campus.

**What makes us different:**
- **40-50% below market rates** — leveraging 7,000+ learners and AI acceleration
- **24-48 hour MVP capability** — Claude Code + trained builders
- **Local team** — Erode-Coimbatore-Tirupur corridor, Tamil & English
- **No vendor lock-in** — you own everything we build

### Five Service Lines

| Service | What | Price Range |
|---------|------|-------------|
| Custom Software | AI-powered apps built in days | ₹3.5-5L |
| JICATE Intelligence | Make your hardware think (cameras, sensors, machines + AI) | ₹75K-25L |
| Corporate AI Training | 4-phase AI transformation for your team | ₹50K-8L |
| Process Documentation + AI ERP | Map your workflows, automate with AI | ₹1.5-15L |
| Content & Media | Videos, social, presentations at 50-70% below market | ₹5K-40K |

### Partner Discounts

50% off for: Yi (Young Indians) members, CII members, JCI members, JKKN alumni companies, MoU partners.

---

**Built with [Claude Code](https://claude.ai/claude-code) by JICATE Solutions**

*JKKN Educational Institutions, Komarapalayam, Namakkal, Tamil Nadu, India*
