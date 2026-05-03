# Contributing to urleval

Thanks for your interest in improving urleval. Contributions are welcome — bug reports, research updates, new test cases, and skill improvements alike.

## Reporting bugs

Open an issue using the [bug report template](.github/ISSUE_TEMPLATE/bug_report.yml). Include the site description and candidate URLs you used, the actual output you got, and what you expected instead. Redact anything sensitive.

## Suggesting improvements

Open an issue using the [feature request template](.github/ISSUE_TEMPLATE/feature_request.yml). Describe the problem you're running into and what you'd like the skill to do differently.

## Making changes

All skill logic lives in a single file: `SKILL.md`. There is no build step.

```bash
git clone https://github.com/keithmackay/urleval ~/.claude/plugins/urleval
cd ~/.claude/plugins/urleval

# Install the skill so you can test changes live
mkdir -p ~/.claude/skills/urleval
ln -sf "$(pwd)/SKILL.md" ~/.claude/skills/urleval/SKILL.md
```

Changes to `SKILL.md` take effect immediately via the symlink — no reinstall needed.

## Testing

Tests are manual invocations inside Claude Code. The full test suite is in [`docs/testing/TEST_CASES.md`](docs/testing/TEST_CASES.md).

Run the test cases affected by your change and record actual outputs in [`docs/testing/SAMPLE_RUNS.md`](docs/testing/SAMPLE_RUNS.md). If you're adding a new behavior, add a corresponding test case.

## Updating the research baseline

If you find new academic or industry research that changes how a dimension should be scored:

1. Add the source and findings to [`docs/research/DOMAIN_NAME_RESEARCH.md`](docs/research/DOMAIN_NAME_RESEARCH.md)
2. Update the matching rubric inside the `<!-- RESEARCH_SUMMARY_START -->` block in `SKILL.md`
3. Update the "Last updated" date in that block
4. Run the affected test cases and note any score changes

## Pull requests

- Branch from `main`: `git checkout -b your-branch-name`
- Keep changes focused — one improvement per PR
- Update `TEST_CASES.md` if you changed skill behavior
- The PR description should explain *why* the change improves the skill, not just *what* changed
