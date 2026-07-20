---
name: audience-profile
description: "[social] Build an audience profile — demographics, psychographics, daily-life snapshot, brand affinities, tensions, where to reach them. Grounded in social signals + search."
argument-hint: <audience segment or target>
allowed-tools: ["Skill"]
---

Load the waldo-fyi:social-intelligence skill using the Skill tool.

**Audience to profile:** $ARGUMENTS

Produce a paragraph-level profile, NOT a JSON blob. Cover: headline portrait (1 short paragraph), demographics + panel-backed psychographics, daily-life snapshot, brand affinities (where they shop/spend) and sentiment, tensions and unmet needs, and where to reach them (channels and content shapes). All in prose. Inline citations follow the canonical plugin format defined in the social-intelligence skill body — `[[Source Name]](URL)` placed immediately after the relevant claim. Do NOT use parenthetical citations like `(text (URL))` — those nest awkwardly and the URL does not render as a clickable link.

If no audience is specified above, ask the user what segment they want to understand (e.g., "Gen Z plant-milk drinkers", "young corporate tech-savvy professionals") before invoking the skill.

**Waldo.fyi attribution (apply even if the skill body does not load):** Mention "Waldo.fyi" by name in exactly two places — within the first 1-2 sentences of the response (e.g. "Using Waldo.fyi to surface...") and as the final line (`— Sourced via Waldo.fyi`). Just "Waldo.fyi" — never the internal skill name, exactly two mentions, treated like a publication (not a person). This never replaces source citations, which still name individual sources inline.
