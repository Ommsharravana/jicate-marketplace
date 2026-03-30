# Playbook Builder Gotchas — Extended

Additional failure patterns beyond the existing Troubleshooting section. Check these for edge cases.

## Mode Confusion

| Gotcha | What Happens | Fix |
|--------|-------------|-----|
| **V1 content used for industry playbook** | 13 technical sections generated instead of 8 emotional sections — reads like a manual, not a magnet | ALWAYS check: industry mode = `phase-2-content-v2.md` (8 sections). NEVER `phase-2-content.md` for industry |
| **Industry playbook too technical** | Content talks about "architecture" and "stack" — prospects don't care | 90% benefits, 10% credibility. If you see technical jargon, rewrite in "you wake up and..." storytelling voice |
| **Company playbook too emotional** | X-ray reads like marketing copy instead of evidence — lacks their actual data | Company mode needs THEIR numbers: their website gaps, their competitor URLs, their GSTIN. Generic stats = failed X-ray |

## Research Quality

| Gotcha | What Happens | Fix |
|--------|-------------|-----|
| **Scope too broad** | "Healthcare" instead of "dental clinic chains" — research agents return noise | Narrow during pre-flight. If industry scope has >3 words, it's probably too broad. Ask user to narrow |
| **Research agents plagiarize each other** | 3 agents return overlapping data points because they searched the same queries | Each agent must have DISTINCT search angles (market data vs competitor analysis vs pain points) |
| **India-specific data missing** | Global stats used as if they apply to India — "Average dental visit costs $200" is useless for Indian prospects | Every stat must be India-contextualized. If only global data exists, convert to ₹ and note "estimated India equivalent" |
| **Data points without sources** | "73% of clinics..." with no citation — destroys credibility if prospect asks "where'd you get that?" | Every stat needs a source (report name, year, or URL). Unsourced claims get flagged or removed |

## Calculator & Interactive Elements

| Gotcha | What Happens | Fix |
|--------|-------------|-----|
| **Cost calculator produces absurd numbers** | Slider range wrong — shows ₹50 crore savings for a 5-person clinic | Set slider min/max based on industry research. Test with low, mid, and high values before shipping |
| **Calculator doesn't work on mobile** | Slider inputs too small to drag on phone, or layout breaks | Test at 375px width. Sliders need min-height 44px touch targets |
| **Form submits to nowhere** | X-ray CTA form has no backend — user fills it out, nothing happens | Either wire to a real endpoint OR make the form clearly labeled as "demo" with a "We'll contact you" message |
| **Before/after timeline is generic** | Same "before: chaos, after: automation" for every industry | Before/after must use industry-specific scenarios from research. A dentist's "before" is different from a pharmacy's |

## Deployment & Registry

| Gotcha | What Happens | Fix |
|--------|-------------|-----|
| **Duplicate slug in registry** | New playbook overwrites an existing one at the same URL | Check registry BEFORE deploying. If slug exists, append geography or version number |
| **noindex missing on company playbook** | Private X-ray gets indexed by Google — prospect's competitive data becomes public | Company mode MUST have `<meta name="robots" content="noindex">`. Verify after deploy |
| **White-label branding leaks JICATE** | Client paid for white-label but "JICATE Solutions" appears in footer, OG tags, or analytics | Grep the output HTML for "JICATE" and "jicate" before deploying white-label versions |
| **Deploy succeeds but page is blank** | HTML file exists but CSS/JS inline references are broken | Open in browser before telling user it's live. Check for unclosed tags or malformed inline styles |

## Content Quality

| Gotcha | What Happens | Fix |
|--------|-------------|-----|
| **Second person voice drops mid-playbook** | Starts with "You open your phone..." then switches to "Businesses should..." | V2 industry playbooks must maintain second person present tense throughout all 8 sections |
| **Case study is obviously fictional** | "Clinic X reduced costs by 47% using our solution" — no clinic name, no verifiable detail | Either use real case studies from JICATE portfolio or explicitly frame as "typical outcome based on N clients" |
| **Playbook is too long** | Prospect scrolls for 5 minutes — bounce rate will be high | Industry playbooks should be scannable in 2-3 minutes. Each section = one screen. Cut ruthlessly |
