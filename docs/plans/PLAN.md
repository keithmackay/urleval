# urleval Skill — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a Claude Code skill (`/urleval`) that evaluates candidate domain names for a website, scores them across 8 dimensions, checks availability, generates alternatives, and produces a structured Markdown report.

**Architecture:** The skill is a single `SKILL.md` file loaded by Claude Code when `/urleval` is invoked. All logic lives in the skill prompt — there is no runtime code to compile or deploy. The skill uses Claude's native reasoning plus web search tool calls for availability checks and research. A static research summary baked into the skill provides domain-naming criteria; the `--update` flag refreshes this from live web searches.

**Tech Stack:** Claude Code skill system (SKILL.md), Markdown, web search (Claude's built-in WebSearch tool), WHOIS/registrar lookup via web search, no external dependencies.

---

## Introduction

### Purpose

This document is a complete, step-by-step implementation guide for the `urleval` Claude Code skill. It assumes the reader is a competent software developer who has never worked with Claude Code skills before and knows little about domain name evaluation theory.

### Audience

A developer executing this plan should be able to start from zero and ship a working, tested skill without asking any clarifying questions.

### Guiding Principles

- **YAGNI** — build exactly what is specified. No extra scoring dimensions, no API integrations, no UI.
- **DRY** — the research summary is written once (in `docs/research/DOMAIN_NAME_RESEARCH.md`) and referenced/copied into the skill. Update in one place.
- **TDD** — test each phase with concrete examples before moving on. For a skill, "tests" are manual invocations with known inputs and expected outputs documented in `docs/testing/`.
- **Frequent commits** — commit after every task that produces a working state.

---

## Technology Stack Overview

### Claude Code Skills

A Claude Code skill is a Markdown file named `SKILL.md` that lives in a skill directory. When Claude Code loads, it reads skill metadata (the YAML front matter: `name` and `description`) from all skills in its search path. When a user invokes `/urleval`, Claude Code loads the full `SKILL.md` and uses its instructions to guide the response.

**Skill search paths (in priority order):**
1. `.claude/skills/<skill-name>/SKILL.md` — project-local skills
2. `~/.claude/skills/<skill-name>/SKILL.md` — user-global skills

For this project, the skill should be installed as a **user-global skill** at:
```
~/.claude/skills/urleval/SKILL.md
```

The project repository at `~/.claude/plugins/urleval/` (or the user's preferred plugin location) is the canonical source; the skill file is copied/symlinked from there.

**SKILL.md structure:**
```
---
name: urleval
description: <one-sentence trigger description for Claude to match invocations>
---

# urleval

[skill body in Markdown]
```

The description is critical — Claude uses it to decide when to invoke the skill. It must mention the trigger phrase `/urleval` and describe what the skill does in ≤ 2 sentences.

### Web Search

Claude Code has a built-in web search capability. Inside a skill, instruct Claude to call web search with specific queries. Results are returned as text summaries that Claude then reasons over. No API keys are needed.

### No Build Step

This project has no package.json, no compilation, no test runner beyond manual invocation. "Tests" in this plan are documented invocation scenarios with expected outputs.

---

## File Map

Before diving into phases, here is every file this project will create or modify:

```
~/.claude/skills/urleval/
└── SKILL.md                          ← the skill (final destination)

docs/
├── research/
│   └── DOMAIN_NAME_RESEARCH.md       ← static research summary (Phase 1)
├── testing/
│   ├── TEST_CASES.md                 ← manual test scenarios (Phase 2+)
│   └── SAMPLE_RUNS.md                ← recorded actual outputs (Phase 3+)
└── plans/
    ├── PLAN.md                       ← this file
    └── PHASES_SUMMARY.md             ← concise summary

README.md                             ← installation + usage (Phase 8)
```

The skill file in `~/.claude/skills/urleval/SKILL.md` is the live install. During development, maintain the source in this repository and copy it on each iteration:

```bash
mkdir -p ~/.claude/skills/urleval
cp ~/.claude/plugins/urleval/SKILL.md ~/.claude/skills/urleval/SKILL.md
# or use a symlink:
ln -sf $(pwd)/SKILL.md ~/.claude/skills/urleval/SKILL.md
```

---

## Phase 1: Research Foundation

**Goal:** Establish an evidence-based set of criteria for evaluating domain names. This becomes the authoritative source for the scoring rubric baked into the skill.

**What you'll produce:** `docs/research/DOMAIN_NAME_RESEARCH.md`

---

### Task 1.1: Set Up the Repository

**Files:**
- Create: `docs/research/` directory
- Create: `docs/testing/` directory
- Modify: `README.md`

- [ ] **Step 1: Create directory structure**

```bash
mkdir -p docs/research docs/testing
```

- [ ] **Step 2: Create a minimal README placeholder**

```bash
cat > README.md << 'EOF'
# urleval

A Claude Code skill that evaluates candidate domain names.

## Status

Under construction. See `docs/plans/PLAN.md`.
EOF
```

- [ ] **Step 3: Initial commit**

```bash
git add -A
git commit -m "chore: scaffold project directories"
```

---

### Task 1.2: Research Domain Name Effectiveness

This is a manual research task. Use Claude Code (or any browser) to search for academic and practitioner literature on what makes a domain name effective. The goal is to fill `docs/research/DOMAIN_NAME_RESEARCH.md` with actionable findings.

**Queries to run (use web search):**

1. `academic research domain name memorability length branding`
2. `what makes a good domain name research study`
3. `domain name length SEO impact research`
4. `typo-squatting homophone domain names consumer confusion research`
5. `TLD trust .com vs alternatives consumer perception`
6. `brand domain name distinctiveness trademark research`
7. `pronounceability domain names word-of-mouth marketing`

**Files:**
- Create: `docs/research/DOMAIN_NAME_RESEARCH.md`

- [ ] **Step 1: Run each query and take notes**

For each query, record: source, key finding, and how it maps to a scoring dimension.

- [ ] **Step 2: Write the research summary**

The document must cover all 8 scoring dimensions defined in the skill. Use this template:

```markdown
# Domain Name Research Summary

## Sources and Methodology

[List sources consulted: academic papers, practitioner guides, SEO studies]

## Key Findings by Dimension

### 1. Memorability
**What the research says:**
- Shorter names are more memorable. Studies suggest ≤ 10 characters is optimal.
- Names with 2–3 syllables are recalled more reliably than 4+ syllable names.
- Pronounceable pseudo-words (like "Google", "Etsy") outperform random letter strings.
- Concrete nouns and action words are more memorable than abstract concepts.

**Recommended scoring:**
- 5: ≤ 8 chars, 1–2 syllables, single word or portmanteau
- 4: ≤ 12 chars, 2–3 syllables
- 3: ≤ 16 chars, 3–4 syllables
- 2: 16–20 chars or 5+ syllables
- 1: > 20 chars, hard to segment visually

### 2. Spelling Reliability
**What the research says:**
- Names with common misspellings (their/there, ie/ei) lose significant organic traffic.
- Homophones cause confusion in voice and word-of-mouth contexts.
- Hyphens increase error rates in manual URL entry.

**Recommended scoring:**
- 5: No plausible misspelling, no homophones, no hyphens, no numbers
- 4: One minor ambiguity but obvious correct form
- 3: One notable typo risk or one homophone
- 2: Multiple typo vectors or hyphenated
- 1: Phonetically ambiguous, homophones, or uses numbers as letters

### 3. Pronunciation Clarity
**What the research says:**
- Names that are easy to say aloud spread via word-of-mouth more effectively.
- Consistent vowel sounds (no silent letters) reduce pronunciation ambiguity.
- Non-English letter combinations confuse English speakers.

**Recommended scoring:**
- 5: Unambiguous pronunciation for target audience
- 4: One minor ambiguity (e.g. hard/soft g)
- 3: Two reasonable pronunciations but one is dominant
- 2: Genuinely ambiguous or requires knowing the "correct" pronunciation
- 1: Most people will mispronounce it on first read

### 4. Associations
**What the research says:**
- Domain names that evoke positive associations increase click-through rates.
- Cultural and linguistic sensitivity matters for international brands.
- Negative or unintended double meanings can go viral for the wrong reasons (e.g. Pen Island).

**Recommended scoring:**
- 5: Strong positive associations relevant to site purpose
- 4: Neutral to mildly positive
- 3: Neutral, no strong associations
- 2: One potentially negative association in a significant market
- 1: Strong negative, offensive, or embarrassing associations

### 5. Cleverness / Brand Fit
**What the research says:**
- Invented words (Google, Zappos, Twilio) are highly trademarkable and differentiable.
- Descriptive names are easier to explain but harder to trademark and protect.
- Name–purpose alignment increases trust and reduces cognitive load.

**Recommended scoring:**
- 5: Memorable wordplay or invented word with clear brand identity potential
- 4: Clever adaptation of real word(s) to context
- 3: Generic but clean
- 2: Forgettable or overly generic
- 1: Inconsistent with brand identity or sounds amateurish

### 6. Relevance to Site Purpose
**What the research says:**
- Users form expectations from domain names before visiting.
- Mismatch between name and content reduces trust and increases bounce rate.
- Keyword-containing domains have diminishing SEO benefit (Google has deprioritized exact-match domains) but still aid user comprehension.

**Recommended scoring:**
- 5: Name immediately communicates site purpose
- 4: Strong implied relevance
- 3: Loosely related or requires explanation
- 2: Unclear connection to purpose
- 1: Misleading or contradicts site purpose

### 7. Competitor Overlap
**What the research says:**
- Similarity to established brands creates trademark liability and consumer confusion.
- Names that echo dominant competitors are harder to rank for SEO.
- Even phonetic similarity to a large brand can trigger legal challenges.

**Recommended scoring:**
- 5: Clearly distinct from all competitors in the space
- 4: Minor phonetic similarity to minor players only
- 3: Some similarity to mid-tier competitors
- 2: Close similarity to a well-known competitor
- 1: Nearly identical to or easily confused with a major competitor

### 8. TLD Appropriateness
**What the research says:**
- .com remains the most trusted TLD globally; users default to typing .com.
- .io has strong recognition in the tech/startup space.
- Country-code TLDs (.co.uk, .ca) signal geographic scope.
- New gTLDs (.app, .dev, .shop) are increasingly accepted; .xyz and .info carry lower trust.

**Recommended scoring:**
- 5: .com available, or .io/.co for tech, .org for nonprofits — well-matched to site type
- 4: Strong secondary TLD well-suited to context
- 3: Acceptable TLD, minor mismatch with site type
- 2: Low-trust TLD (.xyz, .info) or TLD implies wrong geographic scope
- 1: TLD actively undermines credibility (.biz, obscure ccTLD for wrong region)

## Score Weights

Based on research priorities (all weights sum to 1.0):

| Dimension              | Weight |
|------------------------|--------|
| Memorability           | 0.20   |
| Spelling Reliability   | 0.15   |
| Pronunciation Clarity  | 0.10   |
| Associations           | 0.15   |
| Cleverness / Brand Fit | 0.10   |
| Relevance to Purpose   | 0.15   |
| Competitor Overlap     | 0.10   |
| TLD Appropriateness    | 0.05   |

**Overall Score = Σ (dimension_score × weight) × 20** (scales 1–5 range to 20–100)

## Research Caveats

- Most academic research is on brand names generally; domain-specific studies are sparse.
- TLD trust rankings change over time as new TLDs gain adoption.
- Cultural associations vary by target market — adjust accordingly.
- Availability is orthogonal to quality; a perfect name that's taken is not actionable.
```

- [ ] **Step 3: Verify the document covers all 8 dimensions**

Check: Memorability, Spelling Reliability, Pronunciation Clarity, Associations, Cleverness/Brand Fit, Relevance to Purpose, Competitor Overlap, TLD Appropriateness. All 8 must have scoring rubrics and weights.

- [ ] **Step 4: Commit**

```bash
git add docs/research/DOMAIN_NAME_RESEARCH.md
git commit -m "docs: add domain name research summary with 8-dimension scoring rubric"
```

---

## Phase 2: Skill Scaffolding

**Goal:** Create a working skill file that installs correctly, appears in `/` autocomplete, and echoes its inputs back. This proves the plumbing works before adding any real logic.

---

### Task 2.1: Create the Skill Directory and Stub SKILL.md

**Files:**
- Create: `SKILL.md` (project root, symlinked to `~/.claude/skills/urleval/SKILL.md`)

- [ ] **Step 1: Create the skill file**

```bash
cat > SKILL.md << 'EOF'
---
name: urleval
description: Evaluates candidate domain names/URLs for a website. Takes a site description and a list of candidate URLs, scores each across 8 dimensions, checks availability, suggests alternatives, and produces a structured Markdown report. Invoke as /urleval.
---

# urleval — Domain Name Evaluator

## Overview

You are helping the user evaluate candidate domain names for a website.

## Inputs

You need two pieces of information. If not provided in the invocation, ask for them interactively before proceeding:

1. **Site description** — A brief description of the website's purpose, audience, and tone.
2. **Candidate URLs** — A list of domain names to evaluate (e.g. `mysite.com`, `mysite.io`).

**Flags:**
- `--update` — If this flag is present, run a web search for recent research on domain name effectiveness and update your analysis criteria accordingly before scoring. Report what changed.

## Hello World (stub)

For now, simply confirm the inputs received and echo them back:

```
Received:
- Site description: [description]
- Candidates: [list]
- Flags: [any flags]

(Scoring engine not yet implemented)
```
EOF
```

- [ ] **Step 2: Install the skill globally**

```bash
mkdir -p ~/.claude/skills/urleval
# Option A: symlink (recommended for development — edits to SKILL.md are instantly live)
ln -sf "$(pwd)/SKILL.md" ~/.claude/skills/urleval/SKILL.md
# Option B: copy (use this if symlinks cause issues)
# cp SKILL.md ~/.claude/skills/urleval/SKILL.md
```

- [ ] **Step 3: Verify installation**

Restart or reload Claude Code. Type `/` and confirm `urleval` appears in the autocomplete list with the correct description.

If it does not appear:
- Confirm `~/.claude/skills/urleval/SKILL.md` exists: `ls -la ~/.claude/skills/urleval/`
- Confirm the YAML front matter is correct (no tabs, proper `---` delimiters)
- Check Claude Code logs if available

- [ ] **Step 4: Run the hello-world test**

Invoke:
```
/urleval
```

Expected: Claude asks for site description and candidate URLs.

Then provide:
```
Site: a recipe sharing platform for home cooks
Candidates: recipebox.com, homechef.io, cookwith.me
```

Expected output:
```
Received:
- Site description: a recipe sharing platform for home cooks
- Candidates: recipebox.com, homechef.io, cookwith.me
- Flags: (none)

(Scoring engine not yet implemented)
```

- [ ] **Step 5: Document the test result in TEST_CASES.md**

```bash
cat > docs/testing/TEST_CASES.md << 'EOF'
# urleval Test Cases

## Phase 2: Hello World

### TC-001: Basic invocation, no flags
- **Input:** Site = "recipe sharing platform for home cooks"; Candidates = recipebox.com, homechef.io, cookwith.me
- **Expected:** Echoes inputs, notes scoring not yet implemented
- **Status:** [ ] Pass / [ ] Fail

### TC-002: Invocation with --update flag
- **Input:** Same as TC-001, plus `--update`
- **Expected:** Echoes inputs with `--update` noted in flags
- **Status:** [ ] Pass / [ ] Fail

### TC-003: No inputs provided
- **Input:** `/urleval` with nothing else
- **Expected:** Claude asks for site description first, then asks for candidate URLs
- **Status:** [ ] Pass / [ ] Fail
EOF
```

- [ ] **Step 6: Commit**

```bash
git add SKILL.md docs/testing/TEST_CASES.md
git commit -m "feat: add urleval skill stub with hello-world invocation"
```

---

### Task 2.2: Input Parsing — Inline vs Interactive

The skill should handle two invocation modes:

**Mode A — All inline:**
```
/urleval --site "recipe platform for home cooks" recipebox.com homechef.io cookwith.me
```

**Mode B — Interactive (preferred for most users):**
```
/urleval
> What does the website do? [user answers]
> Paste your candidate URLs (one per line or comma-separated): [user answers]
```

**Files:**
- Modify: `SKILL.md`

- [ ] **Step 1: Update SKILL.md input-parsing section**

Replace the Inputs section with:

```markdown
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
```

- [ ] **Step 2: Test both modes**

Mode A test — invoke with inline args and confirm no questions are asked.
Mode B test — invoke bare and confirm questions are asked one at a time.

- [ ] **Step 3: Update TC-001 to TC-003 in TEST_CASES.md with results, add TC-004:**

```
### TC-004: Inline mode
- Input: /urleval --site "recipe sharing platform" recipebox.com homechef.io
- Expected: Proceeds directly, echoes parsed inputs without asking questions
- Status: [ ] Pass / [ ] Fail
```

- [ ] **Step 4: Commit**

```bash
git add SKILL.md docs/testing/TEST_CASES.md
git commit -m "feat: add inline and interactive input parsing to urleval skill"
```

---

## Phase 3: Scoring Engine

**Goal:** Implement the full 8-dimension scoring rubric inside the skill prompt. Claude performs the scoring based on the research summary baked into the skill.

---

### Task 3.1: Bake the Research Summary into the Skill

The research from Phase 1 must be embedded in the skill prompt so Claude has criteria when no live research is available (the common case).

**Files:**
- Modify: `SKILL.md`

- [ ] **Step 1: Add a Research Summary section to SKILL.md**

Copy the scoring rubrics and weights from `docs/research/DOMAIN_NAME_RESEARCH.md` into a new section in `SKILL.md`. Keep it concise — strip prose, keep bullet points and tables. This section is marked with a comment so the `--update` flag knows which section to replace:

```markdown
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
- 1: TLD undermines credibility

### Overall Score Formula

```
Overall = (mem×0.20 + spell×0.15 + pron×0.10 + assoc×0.15 + brand×0.10 + rel×0.15 + comp×0.10 + tld×0.05) × 20
```

Scale: 20–100. Scores above 70 are strong candidates.
<!-- RESEARCH_SUMMARY_END -->
```

- [ ] **Step 2: Add Scoring Instructions section**

```markdown
## Scoring Instructions

For each candidate URL:

1. Strip the TLD and score the name portion for dimensions 1–7.
2. Score dimension 8 (TLD) based on the TLD itself.
3. Score each dimension 1–5 using the rubric above.
4. Provide a brief rationale (1–2 sentences) for each score.
5. Calculate the overall score using the formula.
6. Round overall score to one decimal place.
```

- [ ] **Step 3: Update SKILL.md output section to show scoring format**

```markdown
## Output Format (Scoring Phase — before availability check)

For each candidate, produce an internal scoring table:

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
```

- [ ] **Step 4: Test with sample inputs**

Invoke:
```
/urleval --site "recipe sharing platform for home cooks" recipebox.com homechef.io cookwith.me
```

Expected: Claude produces a scoring table for each of the 3 candidates with numerical scores and rationales. Overall scores should be in the 40–90 range (anything outside this range suggests a rubric problem).

Sanity check the scores manually:
- `recipebox.com` — should score well on relevance, moderate on memorability
- `homechef.io` — strong on associations, good TLD for the space
- `cookwith.me` — creative TLD usage, may score lower on TLD appropriateness

- [ ] **Step 5: Document results in SAMPLE_RUNS.md**

```bash
touch docs/testing/SAMPLE_RUNS.md
# Paste the actual Claude output into this file for reference
```

- [ ] **Step 6: Commit**

```bash
git add SKILL.md docs/testing/SAMPLE_RUNS.md
git commit -m "feat: add 8-dimension scoring engine with research-backed rubric"
```

---

### Task 3.2: Score Sanity Check with Diverse Inputs

Run 3 more test cases to calibrate the scoring rubric before moving on.

**Files:**
- Modify: `docs/testing/TEST_CASES.md`

- [ ] **Step 1: Add test cases TC-005 through TC-007**

```
### TC-005: Long, hyphenated, generic domain
- Site: SaaS project management tool
- Candidates: my-project-management-tool.com
- Expected: Low overall score (< 40), especially low on memorability and spelling reliability

### TC-006: Invented word, .io TLD, tech context
- Site: developer API monitoring service
- Candidates: traxio.io, apipulse.com, watchdog.dev
- Expected: traxio.io scores well on cleverness; apipulse.com on relevance; watchdog.dev on associations

### TC-007: Offensive/problematic associations
- Site: Italian pen retailer
- Candidates: penisland.com
- Expected: Score of 1 on Associations with clear rationale about unintended reading
```

- [ ] **Step 2: Run each test case and record results**

- [ ] **Step 3: If any results are unexpected, adjust rubric wording in SKILL.md**

Common calibration issues:
- Rubric too generous (everything scores 4+): tighten the 5-point criteria
- Rubric too harsh (everything scores 2): loosen the 3-point criteria
- Dimension rationales are too vague: add one concrete example per level

- [ ] **Step 4: Commit with calibration notes**

```bash
git add SKILL.md docs/testing/TEST_CASES.md docs/testing/SAMPLE_RUNS.md
git commit -m "test: add scoring calibration cases and adjust rubric based on results"
```

---

## Phase 4: Availability Checking

**Goal:** For each candidate domain, determine if it is Available, Taken, or Unknown using web search to query WHOIS/registrar data.

---

### Task 4.1: Implement Availability Search

**Files:**
- Modify: `SKILL.md`

- [ ] **Step 1: Add Availability Check section to SKILL.md**

```markdown
## Availability Checking

After scoring all candidates, check availability for each one:

1. For each candidate domain, run a web search:
   - Query: `"[domain.tld]" site:whois.domaintools.com OR site:who.is OR site:instantdomainsearch.com`
   - Alternative query if above is ambiguous: `is [domain.tld] available to register`

2. Parse the result:
   - **Available**: WHOIS shows no registrant, or registrar page shows "available to register"
   - **Taken**: WHOIS shows a registrant, registrar shows "registered" or lists a buy price > $50
   - **Unknown**: Results are ambiguous, blocked, or conflicting

3. Annotate each candidate with its availability status before producing the report.

**Note on accuracy:** Web search availability checks are best-effort. Advise the user to verify at their preferred registrar (Namecheap, Cloudflare Registrar, Google Domains) before purchasing.
```

- [ ] **Step 2: Add availability annotation to internal data format**

Update the Output Format section to show availability column:

```markdown
Each candidate gets: score table + availability status + narrative.
```

- [ ] **Step 3: Test with known-taken domains**

Test inputs:
- `google.com` — must return Taken
- `amazon.com` — must return Taken

Test inputs for known-available (use obviously fake domains):
- `xkzq99notadomainzzz.com` — should return Available

- [ ] **Step 4: Document results and edge cases in TEST_CASES.md**

```
### TC-008: Known-taken domain
- Candidates: google.com
- Expected: Availability = Taken
- Status: [ ] Pass / [ ] Fail

### TC-009: Clearly available domain
- Candidates: xkzq99notadomainzzz.com
- Expected: Availability = Available
- Status: [ ] Pass / [ ] Fail

### TC-010: Ambiguous result
- Candidates: [choose a recently-expired or disputed domain]
- Expected: Availability = Unknown with note to verify manually
- Status: [ ] Pass / [ ] Fail
```

- [ ] **Step 5: Commit**

```bash
git add SKILL.md docs/testing/TEST_CASES.md
git commit -m "feat: add domain availability checking via web search"
```

---

## Phase 5: Alternative Suggestions

**Goal:** Generate up to 10 alternative domain names that are (a) relevant to the site purpose, (b) not already in the candidate list, (c) available to register.

---

### Task 5.1: Implement Alternative Generation

**Files:**
- Modify: `SKILL.md`

- [ ] **Step 1: Add Alternative Generation section to SKILL.md**

```markdown
## Alternative Suggestions

After evaluating all candidates, generate alternative domain suggestions:

### Generation strategy (use all three, then merge and deduplicate):

**A. Claude reasoning — semantic expansion**
Think of 5–10 names that:
- Are related to the site's purpose and audience
- Are shorter or more memorable than the weaker candidates
- Use different linguistic strategies (portmanteau, metaphor, action verb, invented word)

**B. Competitor research**
Run a web search: `top [site category] websites domain names`
- Identify naming patterns competitors use
- Suggest names that follow successful patterns without directly copying

**C. Thesaurus expansion**
For the 2–3 most relevant keywords in the site description, find synonyms and adjacent concepts.
Web search: `synonyms for [keyword]` or use linguistic reasoning.
Combine synonyms with common TLDs to generate candidates.

### Filtering

1. Check availability for each generated alternative (same method as Phase 4).
2. Discard unavailable alternatives.
3. Score each available alternative using the 8-dimension rubric.
4. Keep the top 10 by overall score.
5. If fewer than 10 alternatives pass availability check, include all available ones.

### Output

Alternatives are included in the final score table, visually distinguished with an asterisk (*) or "Alt" label.
```

- [ ] **Step 2: Test alternative generation**

```
/urleval --site "recipe sharing platform for home cooks" recipebox.com
```

Expected:
- At least 5 alternative suggestions generated
- Each alternative has an availability status
- Only Available alternatives appear in the suggestions list
- Alternatives are scored and appear in the score table

- [ ] **Step 3: Add TC-011 and TC-012**

```
### TC-011: Alternative generation
- Site: recipe sharing platform
- Candidates: recipebox.com (only one, so more alternatives are needed)
- Expected: ≥ 5 alternatives generated, all marked Available, all scored
- Status: [ ] Pass / [ ] Fail

### TC-012: All candidates taken edge case
- Site: recipe sharing platform
- Candidates: amazon.com, google.com, facebook.com (all known-taken)
- Expected: Alternatives are generated and available ones are suggested; report notes all candidates are taken
- Status: [ ] Pass / [ ] Fail
```

- [ ] **Step 4: Commit**

```bash
git add SKILL.md docs/testing/TEST_CASES.md
git commit -m "feat: add alternative domain suggestion with availability filtering"
```

---

## Phase 6: Report Generation

**Goal:** Assemble all scoring, availability, and alternative data into the final Markdown report format.

---

### Task 6.1: Implement the Full Report Format

**Files:**
- Modify: `SKILL.md`

- [ ] **Step 1: Define the report structure in SKILL.md**

Replace the stub output section with:

```markdown
## Final Report Format

Produce the report in this exact order:

---

### Section 1: Top 3 Recommendations

List the top 3 candidates (candidates + alternatives combined) by overall score, where availability is Available or Unknown. For each:

```markdown
## Top 3 Domain Recommendations

### 1. [domain.tld] — Score: [X.X]/100
**Availability:** Available ✓ / Unknown ⚠️
**Why this one:** [2–4 sentence narrative: what it does well, why it fits the site description, one caveat if any]

### 2. [domain.tld] — Score: [X.X]/100
...

### 3. [domain.tld] — Score: [X.X]/100
...
```

---

### Section 2: Candidate Narratives

One paragraph per evaluated candidate (from the original list), explaining the score in plain English. Include the overall score. Mention the strongest and weakest dimensions. Do not repeat the table data — add interpretive value.

---

### Section 3: Full Score Table

```markdown
## Full Score Table

| Domain | Mem | Spell | Pron | Assoc | Brand | Rel | Comp | TLD | Overall | Availability |
|--------|-----|-------|------|-------|-------|-----|------|-----|---------|--------------|
| candidate1.com | 4 | 5 | 4 | 3 | 3 | 5 | 4 | 5 | 77.5 | Available ✓ |
| *alternative1.io | 5 | 4 | 5 | 4 | 5 | 4 | 5 | 4 | 84.0 | Available ✓ |
| candidate2.com | 3 | 3 | 3 | 4 | 2 | 3 | 3 | 5 | 58.0 | Taken ✗ |
```

- Candidates from the original list appear first, then alternatives (marked with *)
- Sort by Overall score descending
- Taken domains are included but noted — they may still inform naming direction

---

### Closing Note

End with: "Verify availability at your preferred registrar before purchasing. Availability data is sourced from web search and may not reflect real-time registry status."
```

- [ ] **Step 2: Run the full report test**

```
/urleval --site "recipe sharing platform for home cooks" recipebox.com homechef.io cookwith.me
```

Expected: A complete Markdown report with all three sections, properly formatted, rendering cleanly in Claude Code's terminal.

Check:
- Section 1 has exactly 3 entries (unless fewer than 3 available candidates+alternatives exist)
- Section 2 has one paragraph per original candidate
- Section 3 table includes all candidates + alternatives, sorted by score
- Taken domains appear in Section 3 but not in Section 1
- Closing note is present

- [ ] **Step 3: Paste full output into SAMPLE_RUNS.md**

- [ ] **Step 4: Add TC-013**

```
### TC-013: Full report completeness check
- Site: recipe sharing platform for home cooks
- Candidates: recipebox.com, homechef.io, cookwith.me
- Expected: Report has Section 1 (3 recs), Section 2 (3 narratives), Section 3 (table with 13+ rows including alternatives), closing note
- Status: [ ] Pass / [ ] Fail
```

- [ ] **Step 5: Commit**

```bash
git add SKILL.md docs/testing/TEST_CASES.md docs/testing/SAMPLE_RUNS.md
git commit -m "feat: implement full report format with top 3, narratives, and score table"
```

---

### Task 6.2: Markdown Rendering Check

Claude Code renders Markdown in its terminal output. Tables, headers, and bold text should all render correctly. This task ensures nothing is broken by excessive nesting or unusual characters.

- [ ] **Step 1: Review the output from TC-013 for rendering issues**

Things to check:
- Table borders aligned
- Bold text (`**...**`) rendering correctly
- Headers (`## ...`) creating visual hierarchy
- Checkmarks (✓ ✗ ⚠️) displaying correctly — if not, replace with text (Available / Taken / Unknown)

- [ ] **Step 2: Fix any rendering issues in SKILL.md**

Common fix: if table columns are misaligned, simplify column headers (e.g., "Mem" instead of "Memorability").

- [ ] **Step 3: Commit if changes were made**

```bash
git add SKILL.md
git commit -m "fix: improve Markdown table formatting in report output"
```

---

## Phase 7: --update Flag

**Goal:** When the user passes `--update`, run a web search for recent domain naming research, summarize the findings, and update the `<!-- RESEARCH_SUMMARY_START -->` section in the skill.

---

### Task 7.1: Implement Research Refresh

**Files:**
- Modify: `SKILL.md`

- [ ] **Step 1: Add --update handling to SKILL.md**

```markdown
## --update Flag: Research Refresh

When `--update` is present in the invocation:

1. Run the following web searches before scoring:
   - `domain name effectiveness research [current year]`
   - `best practices domain name branding [current year]`
   - `TLD trust perception study [current year]`
   - `domain name length SEO memorability [current year]`

2. Summarize the findings: identify any changes or new evidence that would affect the scoring rubrics or weights.

3. Produce an "Update Report" at the top of your response:
   ```
   ## Research Update ([date])
   
   **Queries run:** [list]
   
   **New findings:**
   - [Finding 1]: [how it affects scoring]
   - [Finding 2]: [how it affects scoring]
   
   **No significant changes found in:** [list unchanged dimensions]
   
   **Updated criteria applied to this evaluation.**
   ```

4. Apply updated criteria to scoring for this session (do not permanently modify the baked-in summary — the user must manually update SKILL.md to make changes permanent).

5. Note at the end of the Update Report: "To make these updates permanent, edit the RESEARCH_SUMMARY section in SKILL.md or re-run this skill with --update to re-apply on demand."
```

- [ ] **Step 2: Test --update flag**

```
/urleval --update --site "developer API monitoring tool" traxio.io apipulse.com
```

Expected:
- Update Report appears at the top
- At least 4 web searches are run
- Scoring proceeds with any updated criteria noted
- Full report is produced

- [ ] **Step 3: Add TC-014**

```
### TC-014: --update flag
- Input: /urleval --update --site "developer API monitoring tool" traxio.io apipulse.com
- Expected: Update Report at top, 4+ web searches run, scoring proceeds, note about making changes permanent
- Status: [ ] Pass / [ ] Fail
```

- [ ] **Step 4: Commit**

```bash
git add SKILL.md docs/testing/TEST_CASES.md
git commit -m "feat: implement --update flag for live research refresh"
```

---

## Phase 8: Polish and Documentation

**Goal:** Handle edge cases, write the README, and tag the release.

---

### Task 8.1: Edge Case Handling

**Files:**
- Modify: `SKILL.md`

- [ ] **Step 1: Add edge case handling instructions to SKILL.md**

```markdown
## Edge Cases

**No candidates provided:**
If the user provides a site description but no candidates, skip directly to Alternative Suggestions (Phase 5). Generate 10 alternatives from scratch and produce a Top 3 + Score Table report. Inform the user that no candidates were provided so all entries are AI-suggested.

**Single candidate:**
Proceed normally. The Top 3 section will contain 1 original + up to 2 alternatives. Note that only one candidate was evaluated.

**All candidates taken:**
Proceed with scoring all candidates (taken domains still receive scores — they inform naming direction). In Section 1, pull from alternatives only. If no alternatives are available, note this and suggest the user visit a registrar's "similar domains" tool.

**Candidate list is very long (> 20 domains):**
Score all candidates. For availability checking, note that checking > 20 domains via web search may be slow. Proceed unless the user asks to stop.

**Invalid URL format:**
If a candidate contains spaces, special characters (other than hyphens), or is clearly not a domain (e.g., "my site"), note the issue and skip that candidate.
```

- [ ] **Step 2: Test edge cases**

```
### TC-015: No candidates
- Input: /urleval --site "recipe sharing platform"  (no URLs)
- Expected: Generates 10 alternatives, Top 3 from alternatives, note that no candidates were provided
- Status: [ ] Pass / [ ] Fail

### TC-016: Single candidate
- Input: /urleval --site "recipe sharing platform" recipebox.com
- Expected: Top 3 = recipebox.com + 2 best alternatives
- Status: [ ] Pass / [ ] Fail

### TC-017: All taken
- Input: /urleval --site "search engine" google.com bing.com yahoo.com
- Expected: Section 1 uses alternatives only, note that all candidates are taken
- Status: [ ] Pass / [ ] Fail

### TC-018: Invalid URL
- Input: /urleval --site "recipe platform" "my recipe site" recipebox.com
- Expected: "my recipe site" is skipped with a note; recipebox.com is evaluated normally
- Status: [ ] Pass / [ ] Fail
```

- [ ] **Step 3: Run all 18 test cases and record results**

Update `TEST_CASES.md` with Pass/Fail for each.

- [ ] **Step 4: Fix any failures, re-test, commit**

```bash
git add SKILL.md docs/testing/TEST_CASES.md
git commit -m "feat: add edge case handling for no candidates, all taken, single candidate, invalid URLs"
```

---

### Task 8.2: Write the README

**Files:**
- Modify: `README.md`

- [ ] **Step 1: Write a complete README**

```markdown
# urleval

A Claude Code skill that evaluates candidate domain names for a website.

## What it does

`/urleval` scores domain name candidates across 8 research-backed dimensions, checks availability via web search, suggests alternatives, and produces a structured Markdown report with:

- Top 3 recommendations (score, narrative, availability)
- Narrative analysis of all candidates
- Full score table (all candidates + alternatives, sorted by score)

## Installation

### Prerequisites

- Claude Code (any version with skill support)

### Install the skill

```bash
# Clone this repository
git clone https://github.com/kquisp/urleval ~/.claude/plugins/urleval
cd ~/.claude/plugins/urleval

# Install the skill globally
mkdir -p ~/.claude/skills/urleval
ln -sf "$(pwd)/SKILL.md" ~/.claude/skills/urleval/SKILL.md
```

### Verify installation

Restart Claude Code, type `/`, and confirm `urleval` appears in the autocomplete list.

## Usage

### Interactive mode
```
/urleval
```
Claude will ask for your site description and candidate URLs.

### Inline mode
```
/urleval --site "description of your site" candidate1.com candidate2.io candidate3.co
```

### With research refresh
```
/urleval --update --site "description" candidate1.com
```
Runs live web searches for recent domain naming research before scoring.

## Scoring Dimensions

| Dimension              | Weight | What it measures |
|------------------------|--------|-----------------|
| Memorability           | 20%    | Length, syllables, distinctiveness |
| Spelling Reliability   | 15%    | Typo risk, homophones, hyphens |
| Pronunciation Clarity  | 10%    | Ambiguity, silent letters |
| Associations           | 15%    | Positive/negative, cultural |
| Cleverness / Brand Fit | 10%    | Uniqueness, trademark potential |
| Relevance to Purpose   | 15%    | How well name signals site content |
| Competitor Overlap     | 10%    | Similarity to existing brands |
| TLD Appropriateness    | 5%     | TLD trust and context fit |

Overall score: 20–100 (above 70 is strong).

## Research Foundation

Scoring criteria are based on a research summary in `docs/research/DOMAIN_NAME_RESEARCH.md`. Use `--update` to supplement with current web search findings.

## Updating Research

To permanently update the baked-in research summary:

1. Run `/urleval --update` and note the findings
2. Edit `docs/research/DOMAIN_NAME_RESEARCH.md` with the new findings
3. Update the `RESEARCH_SUMMARY` section in `SKILL.md` to match
4. Re-install: `cp SKILL.md ~/.claude/skills/urleval/SKILL.md`
5. Commit and push

## Development

See `docs/plans/PLAN.md` for the full implementation plan and `docs/testing/TEST_CASES.md` for the test suite.
```

- [ ] **Step 2: Commit**

```bash
git add README.md
git commit -m "docs: write complete README with installation, usage, and scoring reference"
```

---

### Task 8.3: Final Review and Tagging

- [ ] **Step 1: Run the full test suite one more time**

Go through all 18 test cases in `TEST_CASES.md`. All should be passing.

- [ ] **Step 2: Review SKILL.md for consistency**

Check:
- Front matter is correct (name, description fields present)
- All sections referenced in PLAN.md are present
- `<!-- RESEARCH_SUMMARY_START -->` and `<!-- RESEARCH_SUMMARY_END -->` comments are present
- No debug stubs or placeholder text remain
- `--update` flag handling is present
- Edge cases section is present
- Report format section is complete

- [ ] **Step 3: Check file sizes**

```bash
wc -l SKILL.md
```

If `SKILL.md` is over 500 lines, consider splitting non-essential content (like edge case rationale) into a separate file linked from the skill. Keep the skill focused on instructions, not explanations.

- [ ] **Step 4: Final commit and tag**

```bash
git add -A
git commit -m "chore: final review pass before v1.0 tag"
git tag v1.0
git push origin main --tags
```

---

## Next Steps

The following enhancements are worth considering after v1.0:

- **[Keith's idea] Persistent research updates** — Add a script that runs `--update` logic and automatically patches `SKILL.md` with new findings, instead of requiring manual editing.

- **[Keith's idea] Batch mode from file** — Support `/urleval --file candidates.txt` to read candidates from a text file, useful when evaluating > 20 domains.

- **[Claude's idea] Score history / comparison** — Store previous evaluations in `~/.claude/urleval-history/` so users can compare runs over time (e.g., "did any previously-taken domains become available?").

- **[Claude's idea] Trademark search** — Add a web search for each candidate against USPTO and EU IPO trademark databases. Flag any that match registered trademarks in the site's category.

- **[Claude's idea] International availability** — For sites targeting non-US markets, also check country-code TLD availability (e.g., `.co.uk`, `.com.au`) alongside the primary TLD.

- **[Claude's idea] Registrar price comparison** — For available domains, run a web search to find the lowest registrar price. Include a "Best Price" column in the score table.

- **[Claude's idea] Social handle availability** — Check Twitter/X, Instagram, and GitHub handle availability for each domain stem (without TLD) using web search.

- **[Keith's idea] Export to CSV** — Produce a CSV version of the score table in addition to Markdown, for sharing with non-Claude Code users.
