url-eval — evaluate candidate domain names/URLs for a website

WHAT IT DOES
  Takes a site description and a list of candidate URLs, scores each
  across 8 research-backed dimensions (memorability, spelling
  reliability, pronunciation, associations, brand fit, relevance,
  competitor overlap, TLD appropriateness), checks live availability via
  web search, generates and scores alternative name suggestions, and
  produces a structured Markdown report with top-3 recommendations, per
  -candidate narratives, and a full score table.

WHAT IT NEEDS
  - A 1-3 sentence site description (purpose, audience, tone)
  - One or more candidate domain names

USAGE
  /url-eval                                    Interactive: prompts for
                                                inputs one at a time
  /url-eval --site "description" url1 url2 ...  Inline mode
  /url-eval --update --site "description" url1  Inline mode with a live
                                                research refresh first
  /url-eval --help                             Show this message and exit

FLAGS
  --site "..."  Site description (purpose, audience, tone)
  --update      Run a web search for recent domain naming research
                before scoring, supplementing the baked-in research
                baseline for this run only
  --help        Show this help message without making any changes
