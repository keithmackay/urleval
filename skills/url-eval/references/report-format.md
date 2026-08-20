# Final Report Format & Edge Cases — Full Reference

Consulted once scoring, availability checking, and alternative generation are complete, to assemble the final report — and for handling non-standard inputs along the way.

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
