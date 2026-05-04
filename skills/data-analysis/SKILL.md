---
name: data-analysis
description: >-
  Analyze structured data files (CSV, Excel, PDF tables, JSON) and datasets.
  Performs calculations, statistical analysis, trend identification,
  period-over-period comparisons, growth-rate math, pivot tables, distribution
  analysis, and data transformations. Use this skill whenever the user provides
  a data file, asks for quantitative analysis, says "crunch the numbers",
  "compare Q3 vs Q4", "what does this data show", "break down these numbers",
  "analyze this CSV", "build a pivot", "calculate growth rate", "find
  outliers", or references files stored in their Waldo workspace
  (spreadsheets, reports, uploaded docs). Also trigger when the user pastes a
  table or asks to reformat tabular data. Do NOT trigger for ad performance
  metrics from third-party platforms (we have files, not API access to ad
  accounts), and do NOT trigger for live brand signals — route those to
  Archival Knowledge.
---

# Data Analysis

Ask clarifying questions ONLY if the request is materially ambiguous. Do NOT ask for confirmation between steps.

## Workflow

1. **Understand** — What metric, comparison, or insight is needed?
2. **Determine** — What data is available? Uploaded directly, pasted in conversation, or stored in a Waldo workspace?
3. **Prepare** — Clean the data per the [data preparation guide](references/data-preparation.md)
4. **Execute** — Run all calculations, then sanity-check: do totals add up? Are percentages 0-100? Do trends match raw data?
5. **Present** — Lead with the key insight. Follow with supporting data as markdown tables (never raw JSON unless explicitly requested).

Only perform analysis the user explicitly asked for. Do NOT proactively suggest additional analyses unless they seem unsure how to proceed.

## Analysis Patterns

See [analysis-patterns.md](references/analysis-patterns.md) for detailed methodology on each pattern.

| Pattern | What to calculate |
|---|---|
| **Comparison** | Absolute values AND relative differences (% change). Rank items. |
| **Trend** | Period-over-period changes. CAGR for long periods. Seasonality, inflection points. |
| **Distribution** | Mean, median, mode, std dev. Outliers beyond 2 std devs. |
| **Correlation** | Correlation coefficients, direction, strength. Caveat: correlation ≠ causation. |
| **Composition** | Each component's share. Both absolute values and percentages. |

## Supported Inputs

- **CSV** — Watch for encoding, delimiter, header issues.
- **Excel** — Check for multiple sheets; ask user which if ambiguous.
- **PDF** — Use `fetch_and_analyze_pdf` to extract data, then process.
- **Structured datasets** — JSON or data pasted in conversation.
- **Workspace files** — Files stored in a Waldo space. Use `feed_get_items` or `search_documents` to locate and retrieve them. See [data-preparation.md](references/data-preparation.md) for the retrieval workflow.

## Skill Deferral

This skill handles quantitative data analysis. Route elsewhere for:
- Paid ad library searches → Ad Intelligence skill
- Social media posts/sentiment → Social Intelligence skill
- General web/news research → Web Research skill
- Archived brand signals/insights (feeds, not files) → Archival Knowledge skill
