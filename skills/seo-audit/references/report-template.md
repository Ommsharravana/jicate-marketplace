# SEO+GEO+AEO Audit — Report Template

## Report Structure

```
1. Executive Summary (1 page)
2. Per-Site Scorecards (1 page each)
3. Competitor Matrix (2-3 pages)
4. Action Plan (ranked by impact)
5. Technical Appendix (for IT team — omit if client has no IT team)
```

## Section 1: Executive Summary

- Overall Digital Health Score (0-100 with letter grade)
- Top 3 wins (highest-scoring categories)
- Top 5 critical fixes (lowest-scoring categories with highest potential impact)
- Competitor position summary (client rank vs top 3 competitors)
- AI visibility snapshot ("cited by N of 3 models for M of K target queries")

## Section 2: Per-Site Scorecards (1 page each)

For each client URL:
- SEO score (0-100)
- GEO score (0-100)
- AEO score (0-100)
- Speed score (from Lighthouse, 0-100)
- Mobile score (from Lighthouse mobile, 0-100)
- Top 3 fixes for this site (highest impact-to-effort ratio)

## Section 3: Competitor Matrix (2-3 pages)

- Client vs top 3 competitors on SEO/GEO/AEO scores
- Query-by-query ranking comparison
- AI citation comparison (which competitor gets cited by which model)
- Content gap analysis (topics competitors cover that client doesn't)

**Footnote:** "Competitor scores are estimates based on lightweight homepage analysis, not full audits."

## Section 4: Action Plan (ranked by impact)

| Priority | Criteria | Timeframe |
|----------|----------|-----------|
| P1 | effort < 2 hours AND impact > 10 score points | Do this week |
| P2 | effort < 8 hours AND impact > 5 score points | Do this month |
| P3 | everything else with impact > 3 points | Plan for next quarter |

Each action includes:
- **What to do** (specific instruction)
- **Which site/page** (URL)
- **Expected impact** (estimated score improvement, e.g., "+15 to SEO score")
- **Difficulty** (hours estimate: 1-2h quick fix, 4-8h moderate, 16-40h project)
- **Responsible audience** (business owner / marketing / IT)

## Section 5: Technical Appendix

- Code snippets for structured data fixes
- Server configuration recommendations
- Detailed Lighthouse reports per page
- GEO citation-worthiness rubric criteria (the 10 scoring criteria)

## Report Content Generation Rules

| Report Element | How It Is Generated |
|----------------|-------------------|
| "Top 3 wins" | The 3 highest-scoring sub-categories from the audit data |
| "Top 5 critical fixes" | The 5 lowest-scoring sub-categories, ranked by estimated traffic/visibility impact |
| "Top 3 fixes per site" | The 3 actions for that site with the highest impact-to-effort ratio |
| Industry-specific recs | Claude analyzes site content, detects industry, applies relevant patterns. No hardcoded rules. |
| Regulatory disclaimer | Auto-included when `regulated_industry: true` in client config |

## Report Adaptation by Client Type

| Client Type | Sections Included | Sections Skipped |
|-------------|-------------------|------------------|
| Multi-site (2+ URLs) | All 5 sections | None |
| Single-site (1 URL) | Exec summary, Scorecard (as main body), Competitor matrix, Action plan, Tech appendix | "Cross-site insights", "Institution-wide" framing |
| No-website (competitor-only) | Competitor matrix, Market opportunity analysis, "Build your site like this" blueprint | Per-site scorecard, Technical appendix |

## Report Length Targets

- Executive summary: 1 page
- Per-site scorecard: 1 page each
- Total: ~20 pages max for 10-site client, ~8 pages for single-site
- If exceeding targets, move detail to appendix

## GEO/AEO Disclaimer (ALWAYS include)

> "GEO/AEO scores are point-in-time observations. AI model responses vary over time and across sessions. Scores reflect citation patterns at time of audit, not guaranteed future performance."
