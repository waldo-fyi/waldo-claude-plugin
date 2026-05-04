# Baseline results — `waldo-fyi` v0.1

| Field | Value |
|---|---|
| Plugin version | v0.1 (current `main`) |
| Plugin commit | `70d88bc31` |
| Test environment | Claude Code, fresh session, plugin loaded via `--plugin-dir` |
| Model | Claude Opus 4.7 with adaptive thinking enabled (matches @goody's PRO-2350 setup) |
| Tester | _fill in_ |
| Date | _fill in_ |

## How to run

In a fresh terminal (separate from any active Claude Code session):

```bash
cd /path/to/waldo-agentic
git checkout main
claude --plugin-dir packages/waldo-claude-plugins/plugins/waldo-strategy
```

For each prompt below:

1. Paste the prompt verbatim into the fresh Claude Code session.
2. Observe which Waldo skill (if any) Claude invokes. The skill name appears in the tool-use trail or system-reminder; you can also ask Claude "which skill did you use?" if it isn't obvious.
3. Score routing per the rubric.
4. Score answer quality 1 / 3 / 5 per the entry's `rating_rubric` (full rubric in `evals.json`).
5. Fill in the row.
6. Move to the next prompt in the SAME session (multi-turn) OR start a fresh session per prompt — note which approach you used in the metadata above.

**Routing rubric** (from `evals/README.md`):

- **Pass** — Primary skill (first in `expected_skills`) fires. No wrong skills fire. For `expected_skills: []`, no Waldo skill fires.
- **Partial** — Primary fires alongside a wrong skill, OR only a complementary skill fires.
- **Fail** — Wrong skill fires, no skill fires when one was expected, or any skill fires when none was expected.

---

## Smoke test (7 prompts) — run these first

These 7 prompts cover all 3 active skills (`web-research`, `social-intelligence`, `ad-intelligence`), both primary-web and primary-social cases, persona and POV special cases, and one negative. Validating the harness is usable.

### 1. `consumer-trends-01`

**Expected:** `web-research` (primary) + `social-intelligence` (complementary)

**Prompt:**
> What are current trends in the non-alcoholic beverage industry among Gen Z?

| Result | |
|---|---|
| Skills fired | web-research |
| Routing score | Pass |
| Quality score | 5 |
| Notes | Both web searches and fetch pages done. Sources cited appropriately. |

---

### 2. `persona-02`

**Expected:** `web-research` (primary) + `social-intelligence` (complementary)

**Prompt:**
> As a brand manager at Liquid Death, what would your biggest concern be heading into Q3?

| Result | |
|---|---|
| Skills fired | none |
| Routing score | Fail |
| Quality score | 3 |
| Notes | The response wasn't grounded in search. It should at least have done some web research on the category to get the latest relevant information for the brand and/ or its category. No citations provided. |

---

### 3. `strategic-pov-01`

**Expected:** `web-research`

**Prompt:**
> Draft a POV on why custom GPTs aren't always the right solution for strategy teams.

| Result | |
|---|---|
| Skills fired | none |
| Routing score | Fail |
| Quality score | 3 |
| Notes | The response wasn't grounded in search, so the result was just internal knowledge. It would be more helpful for the response to be linked to what strategists do on a daily basis, especially their deliverables and output. No citations provided. |

---

### 4. `cultural-social-01`

**Expected:** `social-intelligence` (primary) + `web-research` (complementary)

**Prompt:**
> Analyze fan behavior for Taylor Swift vs. Beyoncé.

| Result | |
|---|---|
| Skills fired | social-intelligence |
| Routing score | Pass |
| Quality score | 5 |
| Notes | No brief clarification was done, which is required for some of the skills. Example posts included in citations. Didn't load web-research to supplement findings. |

---

### 5. `competitive-02`

**Expected:** `web-research` (primary) + `ad-intelligence` (complementary)

**Prompt:**
> What are Visit Lauderdale's competitors doing that we're not?

| Result | |
|---|---|
| Skills fired | web-research |
| Routing score | Pass |
| Quality score | 5 |
| Notes | Primary fired, no wrong skill fired (Pass per rubric). Complementary `ad-intelligence` didn't auto-load — captured as a quality observation, not a routing miss. It could also clarify which competitors the user wants to focus on. Web search and fetch page tools called. |

---

### 6. `influencer-02`

**Expected:** `social-intelligence` (primary) + `web-research` (complementary)

**Prompt:**
> What kinds of influencers are driving the most engagement for skincare brands right now?

| Result | |
|---|---|
| Skills fired | social-intelligence |
| Routing score | Pass |
| Quality score | 5 |
| Notes | Brief clarification should have been done but was missed. Didn't load web-research, but this could come in the follow-up questions. |

---

### 7. `negative-04`

**Expected:** _no Waldo skill should fire_

**Prompt:**
> Write me a Python script to parse a CSV file and print the column names.

| Result | |
|---|---|
| Skills fired | none |
| Routing score | Pass |
| Quality score | N/A — routing only |
| Notes | Pythode code block with instructions provided. It mentions that this is outside scope of the 5 strategy-skills. |

---

## Smoke test summary (fill in after running 7)

| Metric | Value |
|---|---|
| Routing pass rate | 5 / 7 |
| Routing partial rate | 0 / 7 |
| Routing fail rate | 2 / 7 |
| Average quality (where applicable) | 4.33 |
| Most common failure mode | Response not grounded in search — `web-research` skill not loaded, no citations, no brief clarification done. Complementary skills never auto-load even on Pass cases. |

**Decision after smoke test:** Harness validated; routing-layer failure confirmed (2 of 7 prompts answered from training without invoking any Waldo skill, both persona/POV cases). Proceed to Phase 2 (Track A description rewrites). Schedule the full 22 remaining should-trigger + 5 remaining negative prompts as a follow-on baseline pass, distributed across team.

### Secondary observations from smoke test

- **Quality is high when routing is correct (avg 5.00 across the 5 Pass cases).** The fix is routing, not skill-body content.
- **Complementary skills never auto-loaded** in any Pass case — only the primary fired. Worth watching post-Phase 2 to see if description tweaks change this.
- **Brief clarification was skipped** in `cultural-social-01` and `influencer-02` despite the skill bodies asking for it. SKILL.md body issue, not a description issue — separate from Phase 2.
- **Negative test was clean** and Claude explicitly named the 5 strategy skills when declining, confirming plugin descriptions are loading correctly into the system prompt.

---

## Full baseline (34 prompts) — fill in over time

Smoke-test entries above are the canonical results for those IDs; this table is for the remaining 27 entries plus a single-row summary at the bottom.

### Should-trigger (remaining 22 of 28)

| ID | Expected primary | Skills fired | Routing | Quality | Notes |
|---|---|---|---|---|---|
| `consumer-trends-02` | web-research | | | | |
| `campaign-examples-01` | web-research | | | | |
| `campaign-examples-02` | web-research | | | | |
| `campaign-examples-03` | web-research | | | | |
| `persona-01` | web-research | | | | |
| `creative-brief-01` | web-research | | | | |
| `creative-brief-02` | web-research | | | | |
| `desk-research-01` | web-research | | | | |
| `desk-research-02` | web-research | | | | |
| `desk-research-03` | web-research | | | | |
| `desk-research-04` | web-research | | | | |
| `strategic-pov-02` | web-research | | | | |
| `cultural-social-02` | web-research | | | | |
| `competitive-01` | web-research | | | | |
| `competitive-03` | web-research | | | | |
| `influencer-01` | social-intelligence | | | | |
| `influencer-03` | web-research | | | | |
| `ai-innovation-01` | web-research | | | | |
| `ai-innovation-02` | web-research | | | | |
| `account-01` | web-research | | | | |
| `account-02` | web-research | | | | |
| `datapoints-01` | web-research | | | | |

### Should-NOT-trigger (remaining 5 of 6)

| ID | Skills fired | Routing | Notes |
|---|---|---|---|
| `negative-01` | | | |
| `negative-02` | | | |
| `negative-03` | | | |
| `negative-05` | | | |
| `negative-06` | | | |

### Full baseline summary

| Metric | Value |
|---|---|
| Routing pass rate | _x / 34_ |
| Routing partial rate | _x / 34_ |
| Routing fail rate | _x / 34_ |
| Average quality | _x.x / 5_ |
| `web-research` invocation rate | _x / 25 expected_ |
| `social-intelligence` invocation rate | _x / 12 expected_ |
| `ad-intelligence` invocation rate | _x / 3 expected_ |
