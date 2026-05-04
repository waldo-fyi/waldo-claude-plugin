# `waldo-fyi` plugin evals

Evaluation set for measuring whether Claude routes user prompts to the correct Waldo skill, and whether the resulting answer matches expected behavior.

Used by [PRO-2386](https://linear.app/waldofyi/issue/PRO-2386) to capture v0.1 baseline and measure lift from description tuning, slash commands, and the MCP `instructions` field.

## Files

- `evals.json` — 28 should-trigger prompts. For each prompt, the expected Waldo skill(s) (in priority order) and goal criteria.
- `negative.json` — should-not-trigger prompts: general factual / coding / off-topic queries where Claude should answer from training rather than invoking Waldo.
- `README.md` — this file.

## Skills under test

| Skill | When it should fire |
|---|---|
| `ad-intelligence` | Paid creative questions — competitor ads, ad library searches, ad spend, share of voice in paid channels. |
| `social-intelligence` | Organic social — what people are saying, sentiment, influencer activity, fan behavior, viral content. |
| `web-research` | Open-web research, market and brand deep-dives, category landscape, audience and cultural trends, multi-source synthesis. |
| `data-analysis` | Quantitative analysis of structured data — CSV / Excel / PDF tables — usually with a file the user has provided or referenced from a Waldo workspace. |
| `archival-knowledge` | Retrieval of previously captured signals or insights inside a Waldo workspace ("what insights do we have on X", "what's been trending"). |

## Eval entry schema (`evals.json`)

```json
{
  "id": "consumer-trends-01",
  "category": "Consumer Trends & Behaviors",
  "prompt": "What are current trends in the non-alcoholic beverage industry among Gen Z?",
  "expected_skills": ["web-research", "social-intelligence"],
  "rationale": "Time-sensitive trend research; Gen Z signals are most visible on social.",
  "expected_behavior": "Identify 3-5 specific, recent trends supported by examples or data with citations; explain relevance to Gen Z",
  "rating_rubric": {
    "1": "vague or outdated trends",
    "3": "current but shallow",
    "5": "specific, timely, and supported with data or examples"
  }
}
```

### Multi-skill expectations

Most strategy queries benefit from triangulating across more than one skill — e.g. a current-trend question pulls from both `web-research` (category landscape) and `social-intelligence` (live audience signal). `expected_skills` is an ordered list:

- **First entry** = the skill expected to do the heaviest lifting / fire first.
- **Subsequent entries** = complementary skills that *should* be invoked to enrich the answer.

A passing route uses the primary skill at minimum. Hitting the complementary skills is a quality bonus, not required for a route to count as correct.

### When `web-research` fires almost everywhere

Time-sensitive prompts (current trends, brand status, recent events) need fresh data, so `web-research` shows up in nearly every entry. Even personas and POVs default to including `web-research` because the realism of the answer depends on grounding the persona in current category context. Pure null cases (no skill should fire) are reserved for off-topic, definitional, or operational queries — see `negative.json`.

### Tradeoff to bear in mind when reading results

Richer answers (more skills triangulated) are more valuable to the strategist, but each tool call costs latency and credits. The eval scores routing *correctness* — did the right skills fire — not *exhaustiveness*. If Claude hits the primary skill and produces a quality 5 answer without the complementary skills, that's still a pass.

## Negative entry schema (`negative.json`)

```json
{
  "id": "negative-01",
  "prompt": "What's 2+2?",
  "expected_skills": [],
  "rationale": "General factual question; should answer from training, no Waldo skill should fire"
}
```

## Scoring rubric

Each eval gets two scores:

**1. Routing correctness** (pass / partial / fail)

- **Pass:** The primary skill (first entry in `expected_skills`) fires. No incorrect skills fire. For `expected_skills: []`, pass means no Waldo skill fires.
- **Partial:** The primary skill fires *and* a wrong skill fires alongside, OR a complementary skill fires but the primary doesn't.
- **Fail:** A wrong skill fires, no skill fires when one should, or any skill fires when none should.

**2. Answer quality** (1–5 per the entry's `rating_rubric`)

- 1 = vague, generic, or off-topic.
- 3 = on-topic but lacks depth, specificity, or citations.
- 5 = specific, well-cited, matches the goal criteria.

Routing correctness is the primary metric for measuring lift from description tuning. Answer quality is secondary and depends on the underlying tools as well as the description.

## How to run

### Stage 1 — Local (Claude Code `--plugin-dir`)

For each prompt in `evals.json`:

1. Start Claude Code with `--plugin-dir` pointing at the local plugin directory.
2. Paste the prompt verbatim. No system-prompt nudging.
3. Record:
   - Which skill (if any) fired.
   - Whether the answer matches the goal criteria.
   - Score per the rating rubric.
4. Repeat for `negative.json`.

Capture the v0.1 baseline against `main`, then re-run on the v0.2 branch to measure lift.

### Stage 2 — Cowork

Hand off to [PRO-2350](https://linear.app/waldofyi/issue/PRO-2350) for Cowork validation. The same eval set, scored the same way, against a personal-plugin-uploaded build of v0.2.

## Known coverage gaps

`web-research` appears in all 28 entries (primary in 25), `social-intelligence` in 12, `ad-intelligence` in 3 (`competitive-02`, `ai-innovation-01`, `account-02`). `archival-knowledge` and `data-analysis` are not exercised at all by the current set. Worth adding ~6–8 supplemental prompts targeting those last two skills before treating the eval as comprehensive.

Most prompts use one or two skills. Three-skill entries are deliberately rare — the first response should be enough to answer the question, with the user able to ask for more depth as a follow-up. Locking in three skills as expected at first invocation would over-claim the routing target.
