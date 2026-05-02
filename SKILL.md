---
name: urleval
description: Evaluates candidate domain names/URLs for a website. Takes a site description and a list of candidate URLs, scores each across 8 research-backed dimensions, checks availability via web search, suggests alternatives, and produces a structured Markdown report. Invoke as /urleval.
---

# urleval — Domain Name Evaluator

## Overview

You are helping the user evaluate candidate domain names for a website. Follow the steps in this skill exactly and in order.

## Inputs

**If the user provided all inputs inline**, proceed directly to scoring. Inline format:
```
/urleval [--update] [--site "description"] url1 url2 url3 ...
```

**If any input is missing**, collect it interactively in this order:
1. Ask: "What does this website do? (Describe its purpose, audience, and tone in 1–3 sentences.)"
2. Ask: "List your candidate domain names, one per line or comma-separated."

Do not ask for both at once. Ask for site description first, wait for the answer, then ask for candidates.

**Parsing rules:**
- Anything after `--site` and before the next flag is the site description
- Bare words containing a `.` are treated as candidate URLs
- `--update` flag can appear anywhere
- Strip `http://`, `https://`, trailing slashes from all URLs before processing

**Flags:**
- `--update` — Run a web search for recent domain naming research before scoring. See the --update section below.

<!-- RESEARCH_SUMMARY_START -->
## Scoring Criteria (Research-Based)

> Last updated: 2026-05-02. Run with `--update` to refresh from live research.

### Dimensions and Rubrics

**1. Memorability (weight: 0.20)**
- 5: ≤ 8 chars, 1–2 syllables, single word or portmanteau
- 4: ≤ 12 chars, 2–3 syllables
- 3: ≤ 16 chars, 3–4 syllables
- 2: 16–20 chars or 5+ syllables
- 1: > 20 chars, hard to segment visually

**2. Spelling Reliability (weight: 0.15)**
- 5: No plausible misspelling, no homophones, no hyphens, no numbers
- 4: One minor ambiguity but obvious correct form
- 3: One notable typo risk or one homophone
- 2: Multiple typo vectors or hyphenated
- 1: Phonetically ambiguous, homophones, or uses numbers as letters

**3. Pronunciation Clarity (weight: 0.10)**
- 5: Unambiguous pronunciation for target audience
- 4: One minor ambiguity (e.g. hard/soft g)
- 3: Two reasonable pronunciations but one is dominant
- 2: Genuinely ambiguous
- 1: Most people will mispronounce it on first read

**4. Associations (weight: 0.15)**
- 5: Strong positive associations relevant to site purpose
- 4: Neutral to mildly positive
- 3: Neutral, no strong associations
- 2: One potentially negative association in a significant market
- 1: Strong negative, offensive, or embarrassing associations

**5. Cleverness / Brand Fit (weight: 0.10)**
- 5: Memorable wordplay or invented word with clear brand identity potential
- 4: Clever adaptation of real word(s) to context
- 3: Generic but clean
- 2: Forgettable or overly generic
- 1: Inconsistent with brand identity or sounds amateurish

**6. Relevance to Purpose (weight: 0.15)**
- 5: Name immediately communicates site purpose
- 4: Strong implied relevance
- 3: Loosely related
- 2: Unclear connection to purpose
- 1: Misleading or contradicts site purpose

**7. Competitor Overlap (weight: 0.10)**
- 5: Clearly distinct from all competitors in the space
- 4: Minor phonetic similarity to minor players only
- 3: Some similarity to mid-tier competitors
- 2: Close similarity to a well-known competitor
- 1: Nearly identical to or easily confused with a major competitor

**8. TLD Appropriateness (weight: 0.05)**
- 5: .com for general, .io/.co for tech, .org for nonprofits — well-matched
- 4: Strong secondary TLD well-suited to context
- 3: Acceptable TLD, minor mismatch
- 2: Low-trust TLD (.xyz, .info) or wrong geographic scope
- 1: TLD undermines credibility (.biz, obscure ccTLD for wrong region)

### Overall Score Formula

```
Overall = (mem×0.20 + spell×0.15 + pron×0.10 + assoc×0.15 + brand×0.10 + rel×0.15 + comp×0.10 + tld×0.05) × 20
```

Scale: 20–100. Scores above 70 are strong candidates.
<!-- RESEARCH_SUMMARY_END -->

## Scoring Instructions

For each candidate URL:

1. Strip the TLD and score the name portion for dimensions 1–7.
2. Score dimension 8 (TLD Appropriateness) based on the TLD itself.
3. Score each dimension 1–5 using the rubric above.
4. Provide a brief rationale (1–2 sentences) for each score.
5. Calculate the overall score using the formula above.
6. Round overall score to one decimal place.

Produce an internal scoring table for each candidate:

| Dimension              | Score | Rationale |
|------------------------|-------|-----------|
| Memorability           | X     | ...       |
| Spelling Reliability   | X     | ...       |
| Pronunciation Clarity  | X     | ...       |
| Associations           | X     | ...       |
| Cleverness / Brand Fit | X     | ...       |
| Relevance to Purpose   | X     | ...       |
| Competitor Overlap     | X     | ...       |
| TLD Appropriateness    | X     | ...       |
| **Overall**            | **X** |           |

## Availability Checking

After scoring all candidates, check availability for each one:

1. For each candidate domain, run a web search using one of these queries:
   - `"[domain.tld]" whois registrar available`
   - `is [domain.tld] available to register`
   - Search the domain on a registrar site like instantdomainsearch.com, namecheap.com, or cloudflare.com/products/registrar

2. Parse the result:
   - **Available** ✓: WHOIS shows no registrant, or registrar page shows "available to register"
   - **Taken** ✗: WHOIS shows a registrant, registrar shows "registered", or lists a buy/transfer price
   - **Unknown** ⚠️: Results are ambiguous, blocked, or conflicting

3. Annotate each candidate with its availability status before producing the final report.

**Note on accuracy:** Web search availability checks are best-effort. Always verify at your preferred registrar (Namecheap, Cloudflare Registrar, Google Domains) before purchasing.

## Scoring Engine Implementation

The scoring engine is now implemented. For each candidate URL provided, this skill:

1. Parses and validates the input (site description, candidate URLs, flags)
2. Scores all candidates across 8 research-backed dimensions using the rubrics above
3. Calculates weighted overall scores (20–100 scale)
4. Produces individual scoring tables for each candidate showing dimension-by-dimension rationales
5. Ranks candidates by overall score

The full report format (including competitor analysis, alternatives, and export formats) will be added in a later phase. For now, output consists of all individual scoring tables organized by candidate, ranked from highest to lowest overall score, with availability status annotations.
