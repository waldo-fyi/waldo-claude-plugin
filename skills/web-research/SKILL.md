---
name: web-research
description: >-
  Structured deep web research methodology supplementing Claude's built-in web
  search. Provides multi-source research with PDF analysis, webpage screenshot
  capture for visual/UX analysis, and synthesized findings with citations. Use
  this skill for research tasks that benefit from Waldo's specialized tools —
  market research, competitive analysis, brand deep dives, visual/UX audits,
  PDF/report analysis, or multi-source investigations requiring structured
  methodology. Combines Waldo tools with any other available search tools for
  the best results.
---

# Web Research

Every research task follows the same loop: **PLAN -> SEARCH -> EVALUATE -> REDIRECT**. Repeat until done.

## Available Tools

| Tool | Purpose |
|---|---|
| `search_web` | Primary tool. Start every research task here. |
| `fetch_page` | Read full content of a URL as text. Max 5 per batch. Does NOT work on PDFs. |
| `fetch_and_analyze_pdf` | Read and analyze PDFs (whitepapers, earnings reports, SEC filings, academic papers). |
| `fetch_and_analyze_image` | Analyze visual content at a URL (screenshots, ad creatives, logos, product photos). |
| `fetch_and_analyze_video` | Analyze video content at a URL. Only when task explicitly requires video analysis. |
| `screenshot` | Capture a screenshot of a web page. Does NOT analyze — follow up with `fetch_and_analyze_image`. |
| `image_search` | Find images on the web (mood boards, design examples, logo lookups). |

## Workflow

1. **PLAN** — What exactly is being asked? What sub-questions exist? How complex is this? (See [search-strategy.md](references/search-strategy.md) for complexity assessment)
2. **SEARCH** — Run 2-4 parallel searches. Use `fetch_page` on the best results (max 5 per batch). Snippets are rarely enough for synthesis.
3. **EVALUATE** — What do I know now? What's missing? Did anything contradict expectations? Are there untried angles?
4. **REDIRECT** — Keep going (new searches), go deeper (`fetch_page`), or move to output.

Do not include planning in the output. The user sees findings, not process.

## Effort Levels

| Category | Min tool calls | When |
|---|---|---|
| Simple Search | 2-5 | Factual queries, single-source answers |
| Research | 5-20 | Multi-source, comparison, synthesis, strategy |

At ~15 tool calls, begin wrapping up. If you can't find good results after 10+ meaningfully different searches across 3+ angles, conclude the information is not available.

## Output

- **Simple lookups**: Answer directly with inline citations. Keep it succinct.
- **Multi-source research**: Bottom line up front → findings organized by theme → citations woven in.
- **Visual analysis**: Same plus embedded screenshot links.

For detailed search strategy, source evaluation, and visual research methodology, see the reference files:
- [search-strategy.md](references/search-strategy.md) — query construction, angle diversity, source ranking
- [visual-research.md](references/visual-research.md) — screenshot pipeline, visual analysis tips

## Anti-Patterns

- Do not cite from memory. Only cite URLs from the current session's search results.
- Do not describe pages visually from text content alone. Run the screenshot pipeline.
- Do not say "I don't have real-time data." Search immediately.
- Do not offer to search. You were invoked to research. Do it.
- Do not start with flattery or preamble. Lead with findings.

**Citation format:** `[[Source Name]](https://url)` after the sentence. Images: `![Page Name](image_url)`.

## Skill Deferral

This skill handles open web research. Route elsewhere for:
- Paid ad library searches → Ad Intelligence skill
- Social media posts/sentiment → Social Intelligence skill
- Data files or statistical analysis → Data Analysis skill
- Archived brand signals/insights → Archival Knowledge skill
