# SEO+GEO+AEO Audit — Scoring Methodology

## Score Scale

All scores use a 0-100 numeric scale. Letter grades shown in parentheses for non-technical audiences. **Rounding rule:** All scores are rounded to the nearest integer (0.5 rounds up) before grade assignment and display.

| Score Range | Grade | Meaning |
|-------------|-------|---------|
| 90-100 | A | Excellent — industry-leading |
| 80-89 | B | Good — minor improvements possible |
| 70-79 | C | Average — clear improvement opportunities |
| 50-69 | D | Below average — significant work needed |
| 0-49 | F | Poor — critical issues |

## Overall Digital Health Score

**Formula:** Weighted average of three category scores:

| Category | Weight | How Calculated |
|----------|--------|----------------|
| **SEO Score** | 50% | Weighted average: Technical SEO (30%), On-page SEO (30%), Content Quality (20%), Mobile (10%), Speed (10%) |
| **GEO Score** | 25% | Weighted average: Structured data completeness (30%), Citation-worthiness assessment (30%), Fact density (20%), E-E-A-T signals (20%) |
| **AEO Score** | 25% | Citation frequency adjusted for quality (see formula below) |

## Sub-Score Calculations

### SEO Sub-Scores

**Technical SEO:** Derived from Lighthouse audit results (0-100 native scale) + crawl findings:
- HTTPS enabled (binary: 0 or 100)
- Canonical tags present (percentage of pages)
- robots.txt exists and valid (binary)
- sitemap.xml exists and valid (binary)
- No broken internal links (percentage)
- Combined into weighted average

**On-page SEO:** Each page scored on presence + quality, averaged across sampled pages:
- Title tag (present, length 50-60 chars, includes target keyword)
- Meta description (present, length 150-160 chars, includes CTA)
- H1 tag (present, unique per page)
- Heading hierarchy (H1 > H2 > H3 properly nested)
- Image alt text (percentage of images with descriptive alt)
- Internal linking (at least 3 internal links per page)

**Content Quality:**
- Thin pages (< 300 words) as percentage of total
- Duplicate content detection (flagged pages)
- Content freshness (percentage updated within 12 months)
- Word count distribution

**Speed:** Direct from Lighthouse Performance score (0-100)

**Mobile:** Direct from Lighthouse Mobile audit score (0-100)

### GEO Sub-Scores

**Citation-worthiness:** Assessed by Claude against a deterministic rubric. Each page scored on 10 criteria, each worth up to 10 points:

1. Has author byline with credentials (+10)
2. Has >3 statistical claims with cited sources (+10)
3. Has schema.org structured data (appropriate type) (+10)
4. Has publication/update date visible (+10)
5. Uses clear heading hierarchy (H1 > H2 > H3) (+10)
6. Has FAQ or Q&A section (+10)
7. Contains original data/research not found elsewhere (+10)
8. Has source citations or bibliography (+10)
9. Content length >1500 words with substantive depth (+10)
10. Has clear organizational identity (About page, contact info linked) (+10)

Score = sum of applicable criteria out of 100.

**Labeled in report as:** "Rubric-scored by AI — see criteria in appendix."

### AEO Sub-Scores

**Citation frequency formula:**

For each target query, take the **best** citation quality across all models (not the sum). This prevents scores exceeding 100 when multiple models cite the client for the same query.

```
Per-query score = max(quality_weight across all models for that query)
AEO Score = min(100, sum(per_query_scores) / (total_queries x 2.0) x 100)
```

Quality weights:
- Not cited = 0
- Mentioned = 1.0
- Recommended = 1.5
- Primary source = 2.0

**Division-by-zero guard:** If `total_queries` is 0, AEO Score = N/A (not 0, not error). Display as "N/A — no target queries available" in the report.

**Example:** Cited in 5 of 10 queries (best quality per query: 3 mentioned, 1 recommended, 1 primary):
= (3x1.0 + 1x1.5 + 1x2.0) / (10x2.0) x 100
= 6.5/20 x 100
= 32.5

**Contextual note for low scores:** For businesses new to online presence, zero AI citations is typical. An AEO score of 0 reflects opportunity, not failure. Add this framing in the report when AEO < 10.

## Reproducibility Notes

| Score Type | Reproducibility |
|------------|----------------|
| SEO, Speed, Mobile | **Deterministic** — same inputs = same scores |
| GEO citation-worthiness | **Rubric-scored** — consistent results across runs (±5 points variance acceptable) |
| AEO citation frequency | **Empirical but non-deterministic** — AI model responses vary |

The report explicitly labels which scores are deterministic vs observational.

## Score Colors (for reports and dashboard)

| Score Range | Color | Hex |
|-------------|-------|-----|
| 80-100 (A/B) | Green | #22c55e |
| 70-79 (C) | Yellow | #eab308 |
| 50-69 (D) | Orange | #f97316 |
| 0-49 (F) | Red | #ef4444 |
