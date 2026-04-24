---
name: archival-knowledge
description: >-
  Search and retrieve previously captured brand intelligence from stored signals,
  insights, reports, and documents within a Waldo Space. Covers signal feeds
  (brand mentions, owned media, paid ads, category news, management activity,
  audience conversations, trending topics) and insight feeds (brand insights,
  ideas, trend insights). Use this skill whenever the user asks about existing
  brand data, archived signals, "what insights do we have on [brand]", "what's
  been trending", "show me brand mentions", "any signals about [topic]", or
  wants to search previously captured research. Requires an active Waldo
  workspace with configured signal/insight feeds.
---

# Archival Knowledge

Search and retrieve previously captured data from signals, insights, reports, and documents in a Waldo space to support strategists, creatives, and analysts.

## Before You Start

Confirm the request is about **feed-based data** (signals, insights, feed items):
- "what signals do we have," "any insights on [topic]," "what's been trending," "show me brand mentions" → **proceed with this skill**
- "open the spreadsheet," "pull that Excel file," "find the CSV," "analyze the file" → **route to Data Analysis skill**

If unsure, ask the user.

## Workflow

1. Identify the target space and relevant feeds
2. Query signal and insight feeds in parallel using `feed_get_items` — see [feed-guide.md](references/feed-guide.md) for parameters and budget rules
3. Filter for published signals (`filter.status = "published"`) unless user says otherwise
4. Synthesize findings with citations from source URLs
5. Present in markdown with proper citation format: `[[Source Name]](URL)`

## Signal Feeds

Search across all relevant signal feeds in parallel:
- Brand Mentions, Owned Media, Paid Ads, Category News, About Management, Audience Convos, Trending Topics, From Management

Use `select` parameter: `data.content.text`, `data.content.title`, `data.platform.url`

## Insight Feeds

Search all insight feeds in parallel:
- Brand Insights, Ideas, Trends Insights

Use `select` parameter: `data.title`, `data.content`, `data.actionableIdeas`

## Key Rules

- **Always use `select`** — never return raw feed item objects (too large for context)
- **Cap at 15 items per feed** by default. Never exceed 20 without explicit user instruction.
- **Apply keyword filters at query level** — do not fetch broadly and filter mentally
- **Start with 5-7 most relevant feeds**, fan out only if initial results are insufficient
- **Never rely on training data** — only present information from retrieved feed items
- **Cite everything** — `[[Source Name]](URL)` for every claim

See [feed-guide.md](references/feed-guide.md) for detailed token budget management and escalation patterns.

## Skill Deferral

This skill handles archived brand data from Waldo feeds. Route elsewhere for:
- Paid ad library searches → Ad Intelligence skill
- Live social media posts/sentiment → Social Intelligence skill
- General web/news research → Web Research skill
- Data files or statistical analysis → Data Analysis skill
