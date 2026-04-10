---
name: researcher
description: Phase 1 of the research pipeline. Resolves current date, then runs three sub-agents (NEWS_SCOUT, PERSONA_ANALYST, ANGLE_HUNTER) to gather raw research data. Writes output to resources/sessions/[exec-name]/raw-research.md.
---

# Researcher — Phase 1 of 3

This is the first phase of the research pipeline. It gathers raw research for a specific executive and writes a structured output file for the validator to consume in Phase 2.

**Input:** Exec name (from `/research-for [exec]` argument)
**Output:** `resources/sessions/[exec-name]/raw-research.md`
**Next step:** `/validate-research [exec]`

---

## Step 0 — Resolve Current Date

Run the following via the Bash tool:

```bash
date +%Y-%m-%d
```

Set these variables. Use them for **every** date reference in this skill — never use a hardcoded range like "last 6 months":

- `TODAY` = result of the command above
- `WINDOW_30_DAYS` = TODAY minus 30 days (e.g., if TODAY is 2026-02-27, then WINDOW_30_DAYS is 2026-01-28)
- `WINDOW_6_MONTHS` = TODAY minus 180 days (e.g., if TODAY is 2026-02-27, then WINDOW_6_MONTHS is 2025-08-31)

State these three values explicitly in your working context before proceeding. All sub-agents below reference `{WINDOW_30_DAYS}` and `{WINDOW_6_MONTHS}` — these resolve to the values computed here.

---

## Step 1 — Load Exec Profile

The exec name comes from the `/research-for` command argument.

1. Search `resources/exec-profiles/` for a file matching the exec name (case-insensitive, match on first name or full name)
2. Load the profile
3. Extract:
   - **Full name** (for output headers and session folder naming)
   - **Role** (title and domain)
   - **Audience** (who follows them)
   - **Content pillars** (the 3–5 topics this exec owns)
4. Derive the **session folder name**: normalize the exec's full name to lowercase with hyphens (e.g., `Austin Daniel` → `austin-daniel`, `Kahlie Huff` → `kahlie-huff`). This must match the exec profile filename convention.
5. Set **SESSION_FOLDER** = `resources/sessions/[normalized-exec-name]/`
6. Create the session folder if it does not exist

**Context discipline:** This phase operates on exec profile data only. Do not load or reference Voice DNA examples — that data is intentionally reserved for later stages.

---

## Step 2 — Run [SUB-AGENT: NEWS_SCOUT]

**Persona:** Focused news gatherer. Prioritizes timeliness over quantity. Would rather surface 5 genuinely fresh items than 10 padded ones.

**Mandate:** Find 5–8 industry news items, statistics, or articles relevant to this exec's content pillars.

**Time gates:**
- Fresh items: published after `{WINDOW_30_DAYS}` — these are primary candidates
- Supporting context: statistics or foundational data published after `{WINDOW_6_MONTHS}` — clearly labelled as supporting context, not treated as current news

**Sources:** Choose based on the exec's role and content pillars. Examples:
- Tech/AI/SaaS: TechCrunch, The Information, The Verge, Product Hunt
- Sales/GTM/Revenue: Sales Hacker, HubSpot Blog, Gartner reports
- Marketing/Demand gen: MarTech, Content Marketing Institute, Marketing Week
- Operations/Leadership: Harvard Business Review, Inc., McKinsey Insights
- Recruiting/HR: ERE Media, SHRM, HR Brew
- Design/Product/UX: Nielsen Norman Group, Smashing Magazine, UX Collective

**Output section:** `## NEWS_SCOUT FINDINGS`

Format each item as:
```
[DATE] [HEADLINE OR STAT] | Source: [Publication name] | Pillar: [Which content pillar this supports]
```

Organize findings under two sub-headers:
- `### Fresh Items (after {WINDOW_30_DAYS})`
- `### Supporting Context (after {WINDOW_6_MONTHS})`

If fewer than 3 fresh items can be found, note it: `⚠️ Fresh item count limited: [N] found. Consider re-running after more news has accumulated.`

---

## Step 3 — Run [SUB-AGENT: PERSONA_ANALYST]

**Persona:** Content strategist reading the exec's profile for structural signals. Does not search the web — works entirely from the loaded exec profile.

**Mandate:** Extract strategic signals from the exec's Content Pillars section and Voice DNA section to inform what angles are genuinely new vs. what would recycle existing territory.

**Source:** Exec profile only (Content Pillars + Voice DNA sections)

**Task:** Produce three lists:

1. **Topics already documented** — angles, themes, or framings already present in the exec's existing content pillars or Voice DNA examples. These are the avoid list — generating more of the same has zero novelty value.

2. **Hook patterns that work** — patterns visible in the Voice DNA examples (if present): opening moves, rhetorical structures, tones that already resonate with this exec's audience. These are patterns to replicate in new angles.

3. **Content angle gaps** — directions the exec's pillars imply but that no existing examples cover. These are the high-value whitespace — topics where the exec has credibility but hasn't written yet.

**Output section:** `## PERSONA_ANALYST FINDINGS`

Format as three numbered lists:
```
### 1. Topics Already Documented (Avoid Recycling)
- [topic/angle]
- [topic/angle]

### 2. Hook Patterns That Work
- [pattern description]
- [pattern description]

### 3. Content Angle Gaps (High-Value Whitespace)
- [gap description]
- [gap description]
```

---

## Step 4 — Run [SUB-AGENT: ANGLE_HUNTER]

**Persona:** Trend spotter. Cares about what's getting engagement right now. Skeptical of anything that sounds generic or evergreen.

**Mandate:** Find 4–6 viral or controversial takes in the exec's specific niche.

**Sources:**
- Reddit: posts with 50+ upvotes in relevant subreddits
- LinkedIn: posts with 200+ reactions in the exec's niche
- Substack newsletters in the exec's niche

**Time gate:** All items must be after `{WINDOW_30_DAYS}`. Items older than this are not current signal.

**Output section:** `## ANGLE_HUNTER FINDINGS`

Format each item as:
```
[PLATFORM] [TREND OR CONTROVERSY DESCRIPTION] | Engagement: [specific signal, e.g., "847 upvotes on r/sales [ESTIMATED — not pulled from live data]" or "posts on this theme averaging 500+ reactions [ESTIMATED — not pulled from live data]"] | Connection: [how this connects to exec's pillars]
```

**All engagement signals must carry `[ESTIMATED — not pulled from live data]`.** These signals are directional context, not verified metrics. They are still useful for angle selection — label them, don't remove them.

If engagement signals are vague (e.g., "seems popular"), also label them `[Signal: LOW CONFIDENCE]`. Low-confidence items are better flagged than fabricated.

---

## Step 5 — Write raw-research.md

Create the file at `{SESSION_FOLDER}/raw-research.md`.

**YAML header:**
```yaml
---
exec: [Full Name]
date: [TODAY]
today_var: [TODAY]
window_30_days: [WINDOW_30_DAYS]
window_6_months: [WINDOW_6_MONTHS]
pillars: [comma-separated list of content pillar names]
status: RESEARCH_COMPLETE
---
```

Append below the header, in order:
1. The full `## NEWS_SCOUT FINDINGS` section
2. The full `## PERSONA_ANALYST FINDINGS` section
3. The full `## ANGLE_HUNTER FINDINGS` section

---

## Step 6 — Summary to User

Display a brief summary in the conversation:

```
Research complete for [Full Name].

- NEWS_SCOUT: [N] fresh items, [N] supporting context items
- PERSONA_ANALYST: [N] avoid topics, [N] hook patterns, [N] angle gaps identified
- ANGLE_HUNTER: [N] trends/controversies found

File written: {SESSION_FOLDER}/raw-research.md

Review the file, then run: /validate-research [exec-name]
```
