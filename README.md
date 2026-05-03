# urleval

A Claude Code skill that evaluates candidate domain names for a website — scoring them across 8 research-backed dimensions, checking availability, and suggesting alternatives you may not have considered.

## What it does

Upload a site description and a list of candidate URLs, and `/urleval` produces a structured Markdown report with:

- **Top 3 recommendations** — score, narrative, availability status
- **Candidate narratives** — plain-English analysis of each domain you submitted
- **Full score table** — all candidates + alternatives, sorted by overall score

Scoring criteria are derived from academic and industry research on domain name memorability, spelling reliability, brand fit, and more. The `--update` flag triggers a live web search to supplement the baked-in research with current findings before scoring.

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

Claude will ask for your site description, then your candidate URLs — one question at a time.

### Inline mode

```
/urleval --site "description of your site" candidate1.com candidate2.io
```

### With research refresh

```
/urleval --update --site "description" candidate1.com
```

Runs live web searches for recent domain naming research before scoring. Changes apply to the current session only; see [Updating the research baseline](#updating-the-research-baseline) to make them permanent.

## Scoring dimensions

| Dimension              | Weight | What it measures |
|------------------------|--------|-----------------|
| Memorability           | 20%    | Length, syllables, distinctiveness |
| Spelling Reliability   | 15%    | Typo risk, homophones, hyphens |
| Pronunciation Clarity  | 10%    | Ambiguity, silent letters |
| Associations           | 15%    | Positive/negative connotations, cultural fit |
| Cleverness / Brand Fit | 10%    | Uniqueness, trademark potential |
| Relevance to Purpose   | 15%    | How well the name signals site content |
| Competitor Overlap     | 10%    | Similarity to existing brands |
| TLD Appropriateness    | 5%     | TLD trust and context fit |

**Overall score: 20–100.** Scores above 70 are strong candidates.

## Research foundation

Scoring criteria are drawn from a curated research summary in [`docs/research/DOMAIN_NAME_RESEARCH.md`](docs/research/DOMAIN_NAME_RESEARCH.md), which covers 30+ academic and industry sources on memorability, spelling reliability, TLD trust, brand distinctiveness, and more.

Run `/urleval --update` to supplement the baked-in summary with live web searches for current findings. The skill will produce an Update Report at the top of the response describing what, if anything, changed.

## Updating the research baseline

To make research updates permanent:

1. Run `/urleval --update` and note the findings
2. Edit [`docs/research/DOMAIN_NAME_RESEARCH.md`](docs/research/DOMAIN_NAME_RESEARCH.md) with new findings
3. Update the `<!-- RESEARCH_SUMMARY_START -->` section in `SKILL.md` to match
4. If installed via symlink (recommended): no action needed — the symlink always points to the latest file. If installed via copy: `cp SKILL.md ~/.claude/skills/urleval/SKILL.md`
5. Commit and push

## Development

Manual test cases covering all 8 phases of the skill are in [`docs/testing/TEST_CASES.md`](docs/testing/TEST_CASES.md). There is no automated test runner — tests are invoked manually inside Claude Code.

To run a test:

```
/urleval --site "recipe sharing platform for home cooks" recipebox.com homechef.io cookwith.me
```

Compare the output against the expected behavior documented in `TEST_CASES.md`. Record actual outputs in [`docs/testing/SAMPLE_RUNS.md`](docs/testing/SAMPLE_RUNS.md) for regression reference.

To contribute a change:

1. Fork the repo and create a branch
2. Edit `SKILL.md` — all skill logic lives there
3. Run the affected test cases manually and update `TEST_CASES.md` if needed
4. Open a pull request

## License

> [!NOTE]
> No LICENSE file is present in this repository. If you intend to share or open-source this project, add one. [choosealicense.com](https://choosealicense.com) can help you pick the right one.

MIT
