---
name: validator
description: Phase 2 of the research pipeline. Reads raw-research.md, re-resolves current date, then runs FACT_CHECKER and ALIGNMENT_CHECKER sub-agents. Writes validated-research.md on pass, or outputs failure report for re-research on fail.
---

# Validator — Phase 2 of 3

This is the second phase of the research pipeline. It reads the raw research file produced by the Researcher skill and checks it for data quality and brand alignment before allowing angle generation to proceed.

**Input:** `resources/sessions/[exec-name]/raw-research.md`
**Output (pass):** `resources/sessions/[exec-name]/validated-research.md`
**Output (fail):** Failure report in conversation, no file written
**Next step (pass):** `/generate-angles [exec]`
**Next step (fail):** Re-run `/research-for [exec]` with failure notes

---

## Step 0 — Resolve Current Date

Run the following via the Bash tool:

```bash
date +%Y-%m-%d
```

Set these variables (same computation as the Researcher skill — this is a deliberate re-resolution, not inherited state):

- `TODAY` = result of the command above
- `WINDOW_30_DAYS` = TODAY minus 30 days
- `WINDOW_6_MONTHS` = TODAY minus 180 days

Cross-reference these against the values stored in `raw-research.md`'s YAML header (`today_var`, `window_30_days`, `window_6_months`). If the dates differ by more than 1 day, note it: `⚠️ Date drift: research was gathered on [raw date], validating on [today]. Windows adjusted to today's date.`

---

## Step 1 — Read raw-research.md

1. Derive the exec name from the `/validate-research` command argument
2. Normalize to lowercase-with-hyphens to locate the session folder (e.g., `Austin Daniel` → `austin-daniel`)
3. Load `resources/sessions/[exec-name]/raw-research.md`
4. Check the YAML header field `status`:
   - If `status: RESEARCH_COMPLETE` → proceed
   - If missing or any other value → stop and tell the user: `"raw-research.md does not have status: RESEARCH_COMPLETE. Run /research-for [exec] first."`
5. Read all three sub-agent sections: NEWS_SCOUT FINDINGS, PERSONA_ANALYST FINDINGS, ANGLE_HUNTER FINDINGS

---

## Step 2 — Run [SUB-AGENT: FACT_CHECKER]

**Persona:** Skeptical editor checking data quality. Default assumption is that data is imprecise until proven otherwise. Prefers a ⚠️ flag over a false ✅.

**Mandate:** Check every item in NEWS_SCOUT FINDINGS and ANGLE_HUNTER FINDINGS against three quality dimensions.

**Limitation (important):** FACT_CHECKER cannot verify live URLs or confirm a URL returns a real page. It checks recency, attribution specificity, and verifiability signals only — not whether a hyperlink resolves.

### For each NEWS_SCOUT item, evaluate:

1. **Date check** — Does the stated date fall within the correct window?
   - Fresh items: must be after `{WINDOW_30_DAYS}`
   - Supporting context items: must be after `{WINDOW_6_MONTHS}`
   - If the item has no date stated, treat as ⚠️ Flag

2. **Attribution check** — Is the source named and specific?
   - Specific: named publication, named author, named report (e.g., "Gartner Q4 2025 report", "Sarah Chen writing in TechCrunch")
   - Vague: "studies show", "experts say", "recent research", unnamed sources
   - Vague attribution = ⚠️ Flag

3. **Verifiability** — Can this claim be described as independently verifiable?
   - Pass: a specific statistic from a named source, a named event, a published article title
   - Fail: a stat with no source, a claim that sounds fabricated, a "common knowledge" assertion treated as data

### For each ANGLE_HUNTER item, evaluate:

1. **Engagement specificity** — Is the engagement signal concrete?
   - Specific: "847 upvotes on r/sales", "3 Substack newsletters covered this", "LinkedIn post by [Name] got 2,300 reactions"
   - Vague: "seems to be trending", "high engagement", "people are talking about this"
   - Vague = ⚠️ Flag; `[Signal: LOW CONFIDENCE]` items from the researcher already flagged receive automatic ⚠️

### Verdict per item:

| Symbol | Meaning |
|--------|---------|
| ✅ Pass | Meets all checks for its type |
| ⚠️ Flag | Vague or improvable — can be used but noted as weak signal |
| ❌ Fail | Date out of window, unverifiable claim, or fabricated-looking stat |

**Output section:** `## FACT_CHECKER REPORT`

Format:
```
### NEWS_SCOUT Items
[DATE HEADLINE] | Source: [X] | ✅/⚠️/❌ [verdict reason]

### ANGLE_HUNTER Items
[PLATFORM TREND] | ✅/⚠️/❌ [verdict reason]

### Overall: PASS / FAIL
(PASS = zero ❌ items. FAIL = one or more ❌ items.)
```

---

## Step 3 — Run [SUB-AGENT: ALIGNMENT_CHECKER]

**Persona:** Brand strategist ensuring angles are fresh and on-brand. Reads the PERSONA_ANALYST findings as the source of truth for what's already documented.

**Mandate:** Cross-reference the research findings against the exec's documented brand to detect recycled angles and contradictions.

### Check 1 — Recycling detection

Compare each ANGLE_HUNTER and NEWS_SCOUT item against the PERSONA_ANALYST's "Topics Already Documented" list.

- If an item closely matches a topic already in the exec's documented pillars, mark it as ⚠️ Overlap — it can still be used but is not differentiated
- If an item is clearly distinct from documented topics, mark it ✅ Fresh

### Check 2 — Contradiction detection

Compare each ANGLE_HUNTER item against the exec's documented positions and Voice DNA signature moves (if present in the raw-research PERSONA_ANALYST findings).

- If an angle contradicts a position the exec has publicly documented, mark it ❌ Contradicts brand
- If no contradiction detected, mark it ✅ Consistent

**Output section:** `## ALIGNMENT_CHECKER REPORT`

Format:
```
### Recycling Check
[Item description] | ✅ Fresh / ⚠️ Overlap

### Contradiction Check
[Item description] | ✅ Consistent / ❌ Contradicts brand

### Overall: PASS / FAIL
(PASS = zero ❌ items. FAIL = one or more ❌ items.)
```

---

## Step 4a — Both Checkers Pass

If both FACT_CHECKER and ALIGNMENT_CHECKER report overall PASS:

1. Write `resources/sessions/[exec-name]/validated-research.md`
2. YAML header — same structure as raw-research.md with one change:
   ```yaml
   ---
   exec: [Full Name]
   date: [TODAY]
   today_var: [TODAY]
   window_30_days: [WINDOW_30_DAYS]
   window_6_months: [WINDOW_6_MONTHS]
   pillars: [same as raw-research.md]
   status: VALIDATION_PASSED
   ---
   ```
3. Body: copy all raw-research.md content (all three sub-agent sections), then append:
   ```
   ## VALIDATION REPORTS

   [Full FACT_CHECKER REPORT section]

   [Full ALIGNMENT_CHECKER REPORT section]
   ```
4. Each item in the original research sections is annotated inline with its verdict symbol (✅/⚠️/❌)
5. Tell the user:
   ```
   Validation passed for [Full Name].
   File written: resources/sessions/[exec-name]/validated-research.md

   Run: /generate-angles [exec-name]
   ```

---

## Step 4b — Any Checker Fails

If either FACT_CHECKER or ALIGNMENT_CHECKER reports overall FAIL:

1. **Do not write validated-research.md**
2. Output a failure report to the conversation:
   ```
   Validation FAILED for [Full Name].

   ## Items requiring correction:

   [List each ❌ item with:]
   - What it was
   - Why it failed (date out of window / unverifiable / contradicts brand)
   - Specific correction guidance (e.g., "Find a source with a specific date and named publication" or "Replace with an angle that doesn't overlap with exec's existing AI-in-sales content")

   Re-run: /research-for [exec-name]
   (Paste this failure report into the new conversation turn so the researcher sub-agents have correction context.)
   ```
3. No automated re-research. The user controls each phase transition.
