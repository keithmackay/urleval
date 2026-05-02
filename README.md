# urleval

A Claude Code skill that evaluates candidate domain names for a website.

## What it does

`/urleval` scores domain name candidates across 8 research-backed dimensions, checks availability via web search, suggests alternatives, and produces a structured Markdown report with:

- **Top 3 recommendations** — score, narrative, availability status
- **Candidate narratives** — plain-English analysis of each domain you submitted
- **Full score table** — all candidates + alternatives, sorted by overall score

## Installation

### Prerequisites

- [Claude Code](https://claude.ai/code)

### Install

```bash
# Clone this repository
git clone https://github.com/keithmackay/urleval ~/.claude/plugins/urleval
cd ~/.claude/plugins/urleval

# Install the skill globally
mkdir -p ~/.claude/skills/urleval
ln -sf "$(pwd)/SKILL.md" ~/.claude/skills/urleval/SKILL.md
```

### Verify

Restart Claude Code, type `/`, and confirm `urleval` appears in the autocomplete list.

## Usage

### Interactive mode (recommended)
```
/urleval
```
Claude will ask for your site description and candidate URLs.

### Inline mode
```
/urleval --site "description of your site" candidate1.com candidate2.io
```

### With research refresh
```
/urleval --update --site "description" candidate1.com
```
Runs live web searches for recent domain naming research before scoring.

## Scoring dimensions

| Dimension              | Weight | What it measures |
|------------------------|--------|-----------------|
| Memorability           | 20%    | Length, syllables, distinctiveness |
| Spelling Reliability   | 15%    | Typo risk, homophones, hyphens |
| Pronunciation Clarity  | 10%    | Ambiguity, silent letters |
| Associations           | 15%    | Positive/negative connotations, cultural fit |
| Cleverness / Brand Fit | 10%    | Uniqueness, trademark potential |
| Relevance to Purpose   | 15%    | How well name signals site content |
| Competitor Overlap     | 10%    | Similarity to existing brands |
| TLD Appropriateness    | 5%     | TLD trust and context fit |

**Overall score: 20–100.** Scores above 70 are strong candidates.

## Research foundation

Scoring criteria are based on academic and industry research summarized in `docs/research/DOMAIN_NAME_RESEARCH.md`. Use `--update` to supplement with current web search findings.

## Updating the research baseline

1. Run `/urleval --update` and note the findings
2. Edit `docs/research/DOMAIN_NAME_RESEARCH.md` with new findings
3. Update the `RESEARCH_SUMMARY` section in `SKILL.md` to match
4. Re-install: `cp SKILL.md ~/.claude/skills/urleval/SKILL.md` (or the symlink updates automatically)
5. Commit and push

## Development

See `docs/plans/PLAN.md` for the full implementation plan and `docs/testing/TEST_CASES.md` for the test suite.

## License

MIT
