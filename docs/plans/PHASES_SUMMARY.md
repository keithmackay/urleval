# urleval Skill — Phases Summary

## Technology Stack

| Component         | Technology |
|-------------------|------------|
| Skill runtime     | Claude Code skill system (`SKILL.md`) |
| Logic             | Claude reasoning (no compiled code) |
| Availability data | Web search (built-in Claude Code tool) |
| Research refresh  | Web search on demand (`--update` flag) |
| Output format     | Markdown (rendered in Claude Code terminal) |
| Version control   | Git + GitHub |
| Dependencies      | None — no npm, no pip, no build step |

## Key Principles

- **YAGNI** — No APIs, no databases, no UI. The skill is a single Markdown file.
- **DRY** — Research criteria are written once in `docs/research/DOMAIN_NAME_RESEARCH.md` and copied into `SKILL.md`. One source of truth.
- **TDD** — Each phase has documented test cases with inputs, expected outputs, and pass/fail tracking in `docs/testing/TEST_CASES.md`. Test before shipping each phase.
- **Frequent commits** — Every task ends with a commit. No uncommitted work at phase boundaries.

---

## Phase 1: Research Foundation

**Goal:** Establish evidence-based scoring criteria from academic and practitioner literature.

**Tasks:**
1. Scaffold project directories (`docs/research/`, `docs/testing/`)
2. Run 7 targeted web searches on domain name effectiveness
3. Write `docs/research/DOMAIN_NAME_RESEARCH.md` with rubrics for all 8 scoring dimensions and weights

**Key Deliverables:**
- `docs/research/DOMAIN_NAME_RESEARCH.md` with scoring rubrics (1–5 per dimension) and weight table

**Verification:** Document covers all 8 dimensions with numeric scoring levels and weights that sum to 1.0.

---

## Phase 2: Skill Scaffolding

**Goal:** Get a working skill stub that appears in `/` autocomplete and handles input.

**Tasks:**
1. Create `SKILL.md` with YAML front matter and hello-world body
2. Install: `ln -sf $(pwd)/SKILL.md ~/.claude/skills/urleval/SKILL.md`
3. Verify skill appears in Claude Code autocomplete
4. Implement input parsing: inline (`--site`, URL args) and interactive (prompted) modes
5. Write `docs/testing/TEST_CASES.md` with TC-001 through TC-004

**Key Deliverables:**
- `SKILL.md` installed at `~/.claude/skills/urleval/SKILL.md`
- TC-001–004 all passing

**Verification:** `/urleval` prompts for missing inputs; inline args are parsed without prompting.

---

## Phase 3: Scoring Engine

**Goal:** Implement the 8-dimension scoring rubric inside the skill.

**Tasks:**
1. Embed research summary (`<!-- RESEARCH_SUMMARY_START/END -->` block) into `SKILL.md`
2. Add scoring instructions: how Claude should evaluate each dimension
3. Add internal score table format for each candidate
4. Run calibration test cases (TC-005 through TC-007) with diverse inputs
5. Adjust rubric wording if scores seem too high or too low

**Key Deliverables:**
- Scoring rubric + weights embedded in `SKILL.md`
- All 8 dimensions produce plausible scores on test inputs
- TC-005–007 recorded in `SAMPLE_RUNS.md`

**Verification:** `recipebox.com` scores differently from `my-project-management-tool.com`; `penisland.com` scores 1 on Associations.

---

## Phase 4: Availability Checking

**Goal:** Annotate each candidate domain as Available / Taken / Unknown using web search.

**Tasks:**
1. Add availability check instructions to `SKILL.md` (WHOIS/registrar web search queries)
2. Define parsing rules for Available vs. Taken vs. Unknown results
3. Add caveat: "verify at your registrar before purchasing"
4. Test with TC-008 (google.com → Taken), TC-009 (nonsense domain → Available), TC-010 (ambiguous → Unknown)

**Key Deliverables:**
- Availability status annotated on all candidates
- TC-008–010 passing

**Verification:** `google.com` returns Taken; obviously fake domain returns Available.

---

## Phase 5: Alternative Suggestions

**Goal:** Generate up to 10 available alternative domain names and add them to the score table.

**Tasks:**
1. Add Alternative Generation section to `SKILL.md` (3 strategies: Claude reasoning, competitor research, thesaurus)
2. Availability-filter generated alternatives (discard Taken; keep Available/Unknown)
3. Score each alternative using the 8-dimension rubric
4. Cap at top 10 alternatives by score
5. Test TC-011 (≥ 5 alternatives generated) and TC-012 (all candidates taken → alternatives fill report)

**Key Deliverables:**
- Alternative generation logic in `SKILL.md`
- Alternatives distinguished from candidates in output (asterisk or "Alt" label)
- TC-011–012 passing

**Verification:** With one candidate provided, at least 5 available alternatives appear, all scored.

---

## Phase 6: Report Generation

**Goal:** Assemble all data into the final three-section Markdown report.

**Tasks:**
1. Define full report format in `SKILL.md`: Section 1 (Top 3), Section 2 (Narratives), Section 3 (Score Table)
2. Ensure Top 3 only includes Available/Unknown candidates
3. Ensure Taken candidates appear in Section 3 table but not Section 1
4. Add closing note about registrar verification
5. Run TC-013 (full report completeness check)
6. Check Markdown rendering — fix table alignment, emoji rendering issues

**Key Deliverables:**
- Full three-section report produced on invocation
- Tables render cleanly in Claude Code terminal
- TC-013 passing

**Verification:** Report has all three sections; candidates + alternatives appear in score table sorted by overall score descending.

---

## Phase 7: --update Flag

**Goal:** When `--update` is passed, run live web searches for recent research and apply updated criteria to the current evaluation session.

**Tasks:**
1. Add `--update` handling to `SKILL.md` (4 web search queries, Update Report format)
2. Apply updated criteria to scoring for the session (not permanent)
3. Include note about how to make changes permanent (edit `SKILL.md` manually)
4. Test TC-014 (`--update` with real site + candidates)

**Key Deliverables:**
- Update Report section appears at top of output when `--update` is used
- Live research supplements baked-in criteria
- TC-014 passing

**Verification:** `--update` triggers at least 4 web searches; Update Report identifies changed/unchanged dimensions.

---

## Phase 8: Polish and Documentation

**Goal:** Handle edge cases, document installation, and ship v1.0.

**Tasks:**
1. Add edge case handling to `SKILL.md`: no candidates, single candidate, all taken, > 20 candidates, invalid URL format
2. Test TC-015 through TC-018 (edge cases)
3. Write complete `README.md` (installation, usage, scoring reference, update instructions)
4. Final SKILL.md consistency review
5. Run all 18 test cases; all must pass
6. `git tag v1.0 && git push origin main --tags`

**Key Deliverables:**
- All 18 test cases passing
- `README.md` complete with install instructions and usage examples
- `v1.0` tag on `main`

**Verification:** A new developer can install and invoke the skill following only the README, without reading PLAN.md.

---

## Success Criteria

The skill is complete when:

- [ ] `/urleval` appears in Claude Code's autocomplete list after installation
- [ ] Invoking bare (`/urleval`) prompts for site description then candidates
- [ ] Invoking with inline args produces a report without prompting
- [ ] Report always has three sections: Top 3, Narratives, Score Table
- [ ] Top 3 contains only Available/Unknown domains
- [ ] All 8 scoring dimensions appear in the score table
- [ ] Availability is checked for all candidates and alternatives
- [ ] At least 5 alternatives are suggested when scoring conditions allow
- [ ] `--update` flag triggers live research and an Update Report
- [ ] Edge cases (no candidates, all taken, single candidate) are handled gracefully
- [ ] All 18 documented test cases pass
- [ ] README covers installation + usage completely

---

## Post-Launch Maintenance

### When to update the research summary

- Run `/urleval --update` periodically (quarterly) to check for new research
- If new findings significantly change weights or rubric levels, edit `docs/research/DOMAIN_NAME_RESEARCH.md` first, then update the `RESEARCH_SUMMARY` block in `SKILL.md`
- Commit and tag each research update (e.g., `v1.1-research-2026-Q3`)

### When to update the skill

- If Claude Code changes its skill file format, update the YAML front matter
- If the `description` field is not triggering the skill reliably, rephrase it to be more specific about the `/urleval` invocation syntax
- If users report scoring feels off, revisit calibration test cases (TC-005–007) and adjust rubric wording

### Skill file location reminder

- Source of truth: `~/.claude/plugins/urleval/SKILL.md` (this repo)
- Live install: `~/.claude/skills/urleval/SKILL.md` (symlink or copy)
- After any change: re-copy or confirm symlink is intact
