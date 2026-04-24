---
name: ad-intelligence
description: >-
  Search and analyze paid advertising across Meta, Google, and LinkedIn ad
  libraries. Competitive creative analysis, advertiser discovery, and regional
  ad presence mapping. Use this skill whenever the user mentions competitor ads,
  ad creative strategy, paid media presence, advertising trends, ad spend, or
  asks things like "what ads is [brand] running", "show me competitor creatives",
  "how is [brand] advertising", or "compare ad strategies". Also trigger for
  questions about ad libraries, ad formats, or media buying insights.
---

# Ad Intelligence

**NEVER answer ad-related questions from memory or training data.** Always use the ad library tools to retrieve live data. If tools return no results, say so — do not guess, infer, or fabricate ad details. Every ad referenced MUST include a citation.

## Before You Start

1. **Brand name or keyword** — HARD REQUIREMENT. If missing, ask and do not proceed.
2. **Optional parameters** (category, platform, region) — ask ONE round of clarifications if not yet asked in this conversation. If already asked, infer and proceed:
   - **Category**: Infer from brand context (e.g. dental brand → healthcare)
   - **Platform(s)**: Default to all three (Meta, Google, LinkedIn)
   - **Region**: Infer from user location or brand HQ. Default US.

State your inferred assumptions briefly before executing.

## Workflow

1. **Discover** — Resolve the advertiser (Meta page ID via `meta_advertiser_search`, domain for Google, company for LinkedIn)
2. **Search** — Query ad libraries. See [tool-guide.md](references/tool-guide.md) for parameters and budget rules
3. **Analyze** — For creative analysis, use `fetch_and_analyze_image` / `fetch_and_analyze_video`. Max 10 ads per batch, all in parallel
4. **Synthesize** — Present findings with inline citations: `[[Source Name]](adUrl)` for links, `![Source Name](image_url)` for images

**Parallel by default:** Batch all independent searches into a single response. Sequential only when one call's output feeds another.

For competitive analysis patterns (side-by-side, category mapping), see [competitive-patterns.md](references/competitive-patterns.md).

## Platform Coverage

Direct tools exist for **Meta**, **Google**, and **LinkedIn** only. For TikTok, YouTube, X/Twitter, Pinterest, Snapchat, Reddit — fall back to `search_web`.

## Skill Deferral

This skill handles paid advertising only. Route elsewhere for:
- Social media sentiment/organic posts → Social Intelligence skill
- General web/news research → Web Research skill
- Data files or statistical analysis → Data Analysis skill
- Archived brand signals/insights → Archival Knowledge skill
