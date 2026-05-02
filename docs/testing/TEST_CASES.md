# urleval Test Cases

## Phase 2: Hello World / Input Parsing

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

### TC-004: Inline mode
- **Input:** `/urleval --site "recipe sharing platform" recipebox.com homechef.io`
- **Expected:** Proceeds directly, echoes parsed inputs without asking questions
- **Status:** [ ] Pass / [ ] Fail

## Phase 3: Scoring Engine

### TC-005: Long, hyphenated, generic domain
- **Site:** SaaS project management tool
- **Candidates:** my-project-management-tool.com
- **Expected:** Low overall score (< 40), especially low on Memorability and Spelling Reliability
- **Status:** [ ] Pass / [ ] Fail

### TC-006: Invented word, .io TLD, tech context
- **Site:** Developer API monitoring service
- **Candidates:** traxio.io, apipulse.com, watchdog.dev
- **Expected:** traxio.io scores well on Cleverness; apipulse.com on Relevance; watchdog.dev on Associations
- **Status:** [ ] Pass / [ ] Fail

### TC-007: Offensive/problematic associations
- **Site:** Italian pen retailer
- **Candidates:** penisland.com
- **Expected:** Score of 1 on Associations with clear rationale about unintended reading
- **Status:** [ ] Pass / [ ] Fail

## Phase 4: Availability Checking

### TC-008: Known-taken domain
- **Site:** Any
- **Candidates:** google.com
- **Expected:** Availability = Taken ✗
- **Status:** [ ] Pass / [ ] Fail

### TC-009: Clearly available domain
- **Site:** Any
- **Candidates:** xkzq99notadomainzzz.com
- **Expected:** Availability = Available ✓
- **Status:** [ ] Pass / [ ] Fail

### TC-010: Ambiguous result
- **Site:** Any
- **Candidates:** [a recently-expired or unusual domain]
- **Expected:** Availability = Unknown ⚠️ with note to verify manually
- **Status:** [ ] Pass / [ ] Fail

## Phase 5: Alternative Suggestions

### TC-011: Alternative generation
- **Site:** Recipe sharing platform for home cooks
- **Candidates:** recipebox.com (only one — more alternatives are needed)
- **Expected:** ≥ 5 alternatives generated, all Available ✓, all scored; alternatives marked with * in table
- **Status:** [ ] Pass / [ ] Fail

### TC-012: All candidates taken
- **Site:** Recipe sharing platform
- **Candidates:** amazon.com, google.com, facebook.com (all known-taken)
- **Expected:** Alternatives are generated; report notes all candidates are taken; Top 3 section draws from alternatives only
- **Status:** [ ] Pass / [ ] Fail

## Phase 6: Report Format

### TC-013: Full report completeness check
- **Site:** Recipe sharing platform for home cooks
- **Candidates:** recipebox.com, homechef.io, cookwith.me
- **Expected:** 
  - Section 1: Exactly 3 recommendations (or fewer with explanatory note), each with score, availability, and narrative
  - Section 2: One paragraph for each of the 3 original candidates (recipebox.com, homechef.io, cookwith.me)
  - Section 3: Score table with all candidates + all Available alternatives (marked *), sorted by Overall score descending, all 10 columns present
  - Closing note about verifying at a registrar
- **Status:** [ ] Pass / [ ] Fail

## Phase 7: --update Flag

### TC-014: --update flag
- **Input:** `/urleval --update --site "developer API monitoring tool" traxio.io apipulse.com`
- **Expected:** Update Report at top of response (with 4 queries listed), scoring proceeds with any updated criteria noted, full report produced, closing note about making changes permanent
- **Status:** [ ] Pass / [ ] Fail

## Phase 8: Edge Cases

### TC-015: No candidates
- **Input:** `/urleval --site "recipe sharing platform"` (no URLs)
- **Expected:** Generates 10 alternatives, Top 3 from alternatives only, opening note that no candidates were provided
- **Status:** [ ] Pass / [ ] Fail

### TC-016: Single candidate
- **Input:** `/urleval --site "recipe sharing platform" recipebox.com`
- **Expected:** Top 3 = recipebox.com + 2 best alternatives; note that only 1 candidate was provided
- **Status:** [ ] Pass / [ ] Fail

### TC-017: All candidates taken
- **Input:** `/urleval --site "search engine" google.com bing.com yahoo.com`
- **Expected:** All 3 candidates scored, Section 1 uses alternatives only, note that all candidates are taken
- **Status:** [ ] Pass / [ ] Fail

### TC-018: Invalid URL
- **Input:** `/urleval --site "recipe platform" "my recipe site" recipebox.com`
- **Expected:** "my recipe site" skipped with a note; recipebox.com evaluated normally
- **Status:** [ ] Pass / [ ] Fail
