# url-eval

A Claude Code skill that evaluates candidate domain names for a website — scoring them across 8 research-backed dimensions, checking availability, and suggesting alternatives you may not have considered.

## What it does

Upload a site description and a list of candidate URLs, and `/url-eval` produces a structured Markdown report with:

- **Top 3 recommendations** — score, narrative, availability status
- **Candidate narratives** — plain-English analysis of each domain you submitted
- **Full score table** — all candidates + alternatives, sorted by overall score

Scoring criteria are derived from academic and industry research on domain name memorability, spelling reliability, brand fit, and more. The `--update` flag triggers a live web search to supplement the baked-in research with current findings before scoring.

## Installation

### Claude Code

```bash
git clone https://github.com/keithmackay/url-eval ~/.claude/plugins/url-eval
cd ~/.claude/plugins/url-eval

mkdir -p ~/.claude/skills/url-eval
ln -sf "$(pwd)/SKILL.md" ~/.claude/skills/url-eval/SKILL.md
```

Restart Claude Code, type `/`, and confirm `url-eval` appears in the autocomplete list.

### Codex

Add an entry to your marketplace JSON (`~/.agents/plugins/marketplace.json`, create if absent):

```json
{
  "name": "personal",
  "interface": { "displayName": "Personal Plugins" },
  "plugins": [
    {
      "name": "url-eval",
      "source": { "source": "local", "path": "/path/to/url-eval/" },
      "policy": { "installation": "AVAILABLE", "authentication": "ON_INSTALL" },
      "category": "Productivity"
    }
  ]
}
```

Then invoke with `/url-eval`.

### Antigravity

The root `SKILL.md` is natively compatible — no conversion needed.

**Global install** (all workspaces):
```bash
cp -r /path/to/url-eval/ ~/.gemini/antigravity/skills/url-eval/
```

**Workspace install** (current project only):
```bash
cp -r /path/to/url-eval/ .agents/skills/url-eval/
```

Skills are auto-discovered. You can also invoke by name: `/url-eval`.

### Gemini CLI

Gemini CLI installs extensions directly from GitHub:

```bash
gemini extensions install https://github.com/keithmackay/url-eval
```

To update:
```bash
gemini extensions update url-eval
```

The skill is auto-discovered from `GEMINI.md` after installation.

## Usage

### Interactive mode (recommended)

```
/url-eval
```

Claude will ask for your site description, then your candidate URLs — one question at a time.

### Inline mode

```
/url-eval --site "description of your site" candidate1.com candidate2.io
```

### With research refresh

```
/url-eval --update --site "description" candidate1.com
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

Run `/url-eval --update` to supplement the baked-in summary with live web searches for current findings. The skill will produce an Update Report at the top of the response describing what, if anything, changed.

## Updating the research baseline

To make research updates permanent:

1. Run `/url-eval --update` and note the findings
2. Edit [`docs/research/DOMAIN_NAME_RESEARCH.md`](docs/research/DOMAIN_NAME_RESEARCH.md) with new findings
3. Update the `<!-- RESEARCH_SUMMARY_START -->` section in `SKILL.md` to match
4. If installed via symlink (recommended): no action needed — the symlink always points to the latest file. If installed via copy: `cp SKILL.md ~/.claude/skills/url-eval/SKILL.md`
5. Commit and push

## Development

Manual test cases covering all 8 phases of the skill are in [`docs/testing/TEST_CASES.md`](docs/testing/TEST_CASES.md). There is no automated test runner — tests are invoked manually inside Claude Code.

To run a test:

```
/url-eval --site "recipe sharing platform for home cooks" recipebox.com homechef.io cookwith.me
```

Compare the output against the expected behavior documented in `TEST_CASES.md`. Record actual outputs in [`docs/testing/SAMPLE_RUNS.md`](docs/testing/SAMPLE_RUNS.md) for regression reference.

To contribute a change, see [CONTRIBUTING.md](CONTRIBUTING.md).

## Compatibility

| Feature | Claude Code | Codex | Antigravity | Gemini CLI |
|---------|:-----------:|:-----:|:-----------:|:----------:|
| Core skill | ✅ | ✅ | ✅ | ✅ |
| Interactive input prompting | ✅ | ✅ | ✅ | ✅ |
| Inline mode (`--site`, URL args) | ✅ | ✅ | ✅ | ✅ |
| `--update` flag (web search) | ✅ | ✅ | ✅ | ✅ |
| 8-dimension scoring | ✅ | ✅ | ✅ | ✅ |
| Availability checking (web search) | ✅ | ✅ | ✅ | ✅ |
| Alternative suggestions | ✅ | ✅ | ✅ | ✅ |

Legend: ✅ Supported

All features are fully portable — the skill uses no platform-specific metadata, triggers, or tool APIs beyond standard web search.

## References

- **Claude Code Skills:** https://code.claude.com/docs/en/skills
- **Codex Plugins:** https://developers.openai.com/codex/plugins/build
- **Antigravity Skills:** https://antigravity.google/docs/skills
- **Gemini CLI Extensions:** https://github.com/google-gemini/gemini-cli/blob/main/docs/extension.md
- **Agent Skills open standard:** https://agentskills.io/home

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for release history.

## License

[MIT](LICENSE)
