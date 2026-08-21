# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/).

## [Unreleased]

- Add --version flag support, reporting installed version and a best-effort GitHub update check
- Add Changelog section to README linking CHANGELOG.md
- Rename skill to url-eval for kebab-case consistency
- Add --help flag convention (help.md)
- Move scoring rubric and report-format/edge-cases to references/, add check-sync.sh

## [1.0.0] - 2026-08-19

- Initial project setup with README
- Add implementation plan and phases summary
- Add .worktrees/ to .gitignore
- docs: add domain name research summary with 8-dimension scoring rubric
- feat: add urleval skill stub with hello-world invocation and input parsing
- feat: add 8-dimension scoring engine with research-backed rubric
- feat: add domain availability checking via web search
- feat: add alternative domain suggestion with availability filtering
- feat: implement full report format with top 3, narratives, and score table
- fix: clarify report format edge cases and TC-013 completeness criteria
- feat: implement --update flag for live research refresh
- fix: clarify --update session-only scope in closing note
- feat: add edge case handling, complete README, finalize skill
- fix: correct memorability rubric ranges, fix asterisk rendering, fix README symlink note, add --update fallback
- docs: improve README with description, research foundation, and development sections
- docs: add LICENSE, CONTRIBUTING, issue templates, and PR template
- feat: port skill to Codex, Antigravity, and Gemini CLI via skillporter

