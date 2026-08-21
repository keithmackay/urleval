---
name: url-eval
description: Evaluates candidate domain names/URLs for a website. Takes a site description and a list of candidate URLs, scores each across 8 research-backed dimensions, checks availability via web search, suggests alternatives, and produces a structured Markdown report. Invoke as /url-eval.
---

# url-eval — Domain Name Evaluator

## Overview

You are helping the user evaluate candidate domain names for a website. Follow the steps in this skill exactly and in order.

## Inputs

**If the user provided all inputs inline**, proceed directly to scoring. Inline format:
```
/url-eval [--update] [--site "description"] url1 url2 url3 ...
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
- `--help` — Do not run the scoring workflow. Instead, read and display the contents of `help.md` (in this skill's folder) verbatim, then stop.
- `--version` — Do not run the scoring workflow. See the --version section below.

### `--version`

If the user invokes this skill with a `--version` flag (e.g. `/url-eval --version`), do not run the workflow. Instead:

1. Read the installed version from this skill's own manifest: `.claude-plugin/plugin.json` if present, else `.codex-plugin/plugin.json`, else `gemini-extension.json` — whichever exists for this platform install. If none exist (a bare Claude Code skill with only SKILL.md), read the topmost version heading in `CHANGELOG.md` instead.
2. Print: `url-eval v<installed-version>`
3. Best-effort update check — determine this skill's GitHub source repo:
   a. If `.git` exists here and `git remote get-url origin` resolves to a `github.com` URL, use that `owner/repo`.
   b. Otherwise, search this skill's own `README.md` for the first `https://github.com/<owner>/<repo>` URL and use that.
   c. If neither yields a repo, or the `gh` CLI isn't installed/authenticated: stop here. Print nothing further — no status line, no error.
4. If a repo was found: run `gh api repos/<owner>/<repo>/releases/latest -q .tag_name` (strip a leading `v`). Compare to the installed version:
   - Equal → append: `Status: up to date`
   - Installed is older → append: `Status: newer version available (v<latest>). To update: if you installed this via a Claude Code marketplace, run /plugin marketplace update <marketplace-name> then reinstall; otherwise, git pull in your install directory if it's a git checkout, or re-copy from https://github.com/<owner>/<repo> per this README's Installation section.`
   - Installed is newer → append: `Status: ahead of latest release (development checkout)`
   - If the API call fails for any reason (network, auth, rate limit, malformed tag): print nothing further — no status line, no error shown to the user.
5. Stop — do not proceed to run the skill's actual workflow.

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

4. Apply any updated criteria to scoring for this session only. Do not modify the baked-in `<!-- RESEARCH_SUMMARY_START -->` section in `references/scoring-rubric.md` — the baked-in summary remains as the default for future runs.

5. End the Update Report with:
   > These updated criteria apply to this session only. To make them permanent, edit the `<!-- RESEARCH_SUMMARY_START -->` section in `references/scoring-rubric.md`, or re-run with `--update` to re-apply on demand.

Read `references/scoring-rubric.md` (in this skill's folder) for the full 8-dimension scoring rubric and overall score formula — that file is the one to edit if updating the research baseline (see the `--update` flag above and CONTRIBUTING.md).

## Scoring Instructions

For each candidate URL:

1. Strip the TLD and score the name portion for dimensions 1–7.
2. Score dimension 8 (TLD Appropriateness) based on the TLD itself.
3. Score each dimension 1–5 using the rubric in `references/scoring-rubric.md`.
4. Provide a brief rationale (1–2 sentences) for each score.
5. Calculate the overall score using the formula in `references/scoring-rubric.md`.
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

## Final Report Format & Edge Cases

Once you have: (a) scored all candidates, (b) checked availability, (c) generated and scored alternatives — read `references/report-format.md` (in this skill's folder) for the exact section-by-section report template (Top 3 Recommendations, Candidate Narratives, Full Score Table, Closing Note) and for how to handle non-standard inputs (no candidates provided, a single candidate, all candidates taken, more than 20 candidates, invalid URL format).
