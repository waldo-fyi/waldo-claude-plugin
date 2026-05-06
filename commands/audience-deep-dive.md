---
name: audience-deep-dive
description: Paragraph-level audience portrait with demographics, psychographics, daily-life snapshot, brand affinities, tensions, and where to reach them — grounded in social signals and search data.
argument-hint: <audience segment or target>
allowed-tools: ["Skill"]
---

Load the waldo-fyi:social-intelligence skill using the Skill tool.

**Audience to portray:** $ARGUMENTS

Produce a paragraph-level portrait, NOT a JSON blob. Cover: headline portrait (1 short paragraph), demographics + panel-backed psychographics, daily-life snapshot, brand affinities (where they shop/spend) and sentiment, tensions and unmet needs, and where to reach them (channels and content shapes). All in prose, with inline citations.

If no audience is specified above, ask the user what segment they want to understand (e.g., "Gen Z plant-milk drinkers", "young corporate tech-savvy professionals") before invoking the skill.
