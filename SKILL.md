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

## --update Flag: Research Refresh

When `--update` is present in the invocation, run this procedure **before scoring**:

1. Run the following web searches:
   - `domain name effectiveness research [current year]`
   - `best practices domain name branding [current year]`
   - `TLD trust perception study [current year]`
   - `domain name length SEO memorability [current year]`

2. Summarize the findings: identify any changes or new evidence that would affect the scoring rubrics or weights compared to the baked-in research summary. If searches return no relevant results or only repeat known findings, state "No significant changes found since [last updated date]" in the Update Report and proceed using the baked-in rubric unchanged.

3. Produce an **Update Report** at the very top of your response (before Top 3 Recommendations):

   ```
   ## Research Update ([today's date])
   
   **Queries run:** [list the 4 queries]
   
   **New findings:**
   - [Finding]: [how it affects scoring, if at all]
   
   **No significant changes found in:** [list dimensions where research is unchanged]
   
   **Updated criteria applied to this evaluation.**
   ```

4. Apply any updated criteria to scoring for this session only. Do not modify the baked-in `<!-- RESEARCH_SUMMARY_START -->` section — the baked-in summary remains as the default for future runs.

5. End the Update Report with:
   > These updated criteria apply to this session only. To make them permanent, edit the `<!-- RESEARCH_SUMMARY_START -->` section in SKILL.md, or re-run with `--update` to re-apply on demand.

<!-- RESEARCH_SUMMARY_START -->
## Scoring Criteria (Research-Based)

> Last updated: 2026-05-02. Run with `--update` to refresh from live research.

### Dimensions and Rubrics

**1. Memorability (weight: 0.20)**
- 5: 6–10 chars, 1–2 syllables, single word or portmanteau
- 4: 11–14 chars, 2–3 syllables
- 3: 15–17 chars, 3–4 syllables, or < 6 chars (too abbreviated to be meaningful)
- 2: 18–22 chars or 5+ syllables
- 1: > 22 chars, hard to segment visually

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

## Alternative Suggestions

After checking availability for all candidates, generate alternative domain name suggestions.

### Generation strategy (use all three, then merge and deduplicate):

**A. Claude reasoning — semantic expansion**
Think of 5–10 names that:
- Are related to the site's purpose and audience
- Are shorter or more memorable than the weaker candidates
- Use different linguistic strategies: portmanteau, metaphor, action verb, invented word, abbreviation

**B. Competitor research**
Run a web search: `top [site category] websites domain names`
- Identify naming patterns competitors use (e.g., short invented words, verbs, metaphors)
- Suggest names that follow successful patterns without directly copying

**C. Thesaurus expansion**
For the 2–3 most relevant keywords in the site description, find synonyms and adjacent concepts.
Use web search: `synonyms for [keyword]` or reason linguistically.
Combine promising terms with common TLDs (.com, .io, .co, .app) to generate candidates.

### Filtering

1. Check availability for each generated alternative (same method as Availability Checking above).
2. Discard unavailable alternatives.
3. Score each available alternative using the 8-dimension rubric.
4. Keep the top 10 by overall score.
5. If fewer than 10 alternatives pass the availability check, include all available ones.

### Output

Include alternatives in the final score table, labeled with "(alt)" after the domain name to distinguish them from the original candidates.

## Final Report Format

Once you have: (a) scored all candidates, (b) checked availability, (c) generated and scored alternatives — produce the report in this exact order:

---

### Section 1: Top 3 Recommendations

List the top 3 domains (from candidates + alternatives combined) by overall score, where availability is Available ✓ or Unknown ⚠️. Taken domains are excluded from Top 3.

Format each entry as:

```markdown
## Top 3 Domain Recommendations

### 1. [domain.tld] — Score: [X.X]/100
**Availability:** Available ✓  
**Why this one:** [2–4 sentences: what it does well, why it fits the site description, one caveat if any]

### 2. [domain.tld] — Score: [X.X]/100
**Availability:** Available ✓ / Unknown ⚠️  
**Why this one:** [2–4 sentences]

### 3. [domain.tld] — Score: [X.X]/100
**Availability:** Available ✓ / Unknown ⚠️  
**Why this one:** [2–4 sentences]
```

If fewer than 3 Available/Unknown domains exist (candidates + alternatives combined), include as many as exist and begin with: 'Note: fewer than 3 available domains were found.'

---

### Section 2: Candidate Narratives

One paragraph per original candidate (from the user's list), explaining the score in plain English. Include the overall score. Mention the strongest and weakest dimensions. Do not repeat the table — add interpretive context.

If any alternative scores notably higher (5+ points overall) than the best original candidate, call it out explicitly: 'Note: [alternative] scores significantly higher than the best candidate and may be worth prioritizing.'

---

### Section 3: Full Score Table

```markdown
## Full Score Table

| Domain | Mem | Spell | Pron | Assoc | Brand | Rel | Comp | TLD | Overall | Availability |
|--------|-----|-------|------|-------|-------|-----|------|-----|---------|--------------|
| candidate1.com | 4 | 5 | 4 | 3 | 3 | 5 | 4 | 5 | 77.5 | Available ✓ |
| alternative1.io (alt) | 5 | 4 | 5 | 4 | 5 | 4 | 5 | 4 | 84.0 | Available ✓ |
| candidate2.com | 3 | 3 | 3 | 4 | 2 | 3 | 3 | 5 | 58.0 | Taken ✗ |
```

- Candidates from the original list appear first, then alternatives (labeled with "(alt)")
- Sort all rows by Overall score descending
- Include all candidates (even Taken ones) and all Available alternatives

---

### Closing Note

End every report with:

> Verify availability at your preferred registrar before purchasing. Availability data is sourced from web search and may not reflect real-time registry status.

## Edge Cases

**No candidates provided:**
If the user provides a site description but no candidates, skip directly to Alternative Suggestions. Generate 10 alternatives from scratch and produce a Top 3 + Score Table report. Begin the response with: "No candidate URLs were provided. Here are AI-suggested alternatives based on your site description:"

**Single candidate:**
Proceed normally. The Top 3 section will contain the 1 original candidate + up to 2 alternatives (if available). Note: "Only one candidate was provided; remaining Top 3 slots are filled from alternatives."

**All candidates taken:**
Score all candidates normally (taken domains still receive scores — they inform naming direction). For Section 1 (Top 3), draw from alternatives only. If no alternatives are available either, note: "All candidates are taken and no available alternatives were found. Consider visiting a registrar's 'similar domains' tool for suggestions."

**More than 20 candidates:**
Score all candidates. For availability checking, note at the start: "Checking availability for [N] domains via web search — this may take a moment." Proceed with all checks.

**Invalid URL format:**
If a candidate contains spaces, special characters (other than hyphens), or is clearly not a domain (e.g., "my site", "https://"), note: "[candidate] doesn't look like a domain name and will be skipped." Continue processing valid candidates.
