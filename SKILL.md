---
name: urleval
description: Evaluates candidate domain names/URLs for a website. Takes a site description and a list of candidate URLs, scores each across 8 research-backed dimensions, checks availability via web search, suggests alternatives, and produces a structured Markdown report. Invoke as /urleval.
---

# urleval — Domain Name Evaluator

## Overview

You are helping the user evaluate candidate domain names for a website. Follow the steps in this skill exactly and in order.

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

**Flags:**
- `--update` — Run a web search for recent domain naming research before scoring. See the --update section below.

## Hello World (stub — scoring not yet implemented)

Confirm the inputs received and echo them back:

```
Received:
- Site description: [description]
- Candidates: [list]
- Flags: [any flags, or "(none)"]

(Scoring engine not yet implemented)
```
