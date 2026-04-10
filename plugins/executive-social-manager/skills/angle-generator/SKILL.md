---
name: angle-generator
description: Phase 3 of the research pipeline. Reads validated-research.md once, then produces 7 distinct typed angle tickets in a single pass. Each angle has a fixed type mandate. Outputs angle tickets to the conversation.
---

# Angle Generator — Phase 3 of 3

This is the third and final phase of the research pipeline. It reads validated research and produces up to 7 distinct, typed angle tickets — each ready to hand off to `/draft`.

**Input:** `resources/sessions/[exec-name]/validated-research.md`
**Output:** Up to 7 angle tickets displayed in the conversation
**Next step:** `/draft [exec] "[angle title]"` for any angle you want to develop

---

## Step 0 — Resolve Current Date

Run via Bash:

```bash
date +%Y-%m-%d
```

Set `TODAY` = result. Use this for any "why now" framing.

---

## Step 1 — Read validated-research.md

1. Derive exec name from the `/generate-angles` command argument
2. Normalize to lowercase-with-hyphens to locate session folder
3. Load `resources/sessions/[exec-name]/validated-research.md`
4. Check YAML header `status`:
   - `VALIDATION_PASSED` → proceed
   - Anything else → stop: `"validated-research.md does not have status: VALIDATION_PASSED. Run /validate-research [exec] first."`
5. Read all research sections and validation reports

---

## Step 2 — Generate All Angles in One Pass

Read the validated research once. For each angle type below, produce one angle that strictly follows its type mandate. Work through all 7 types sequentially in a single generation pass — do not re-read the research for each type.

**Rules:**
- Each angle must stay strictly within its type. A DATA-STORY that produces a contrarian take has failed.
- If the research genuinely cannot support a given type, output `[NO STRONG MATCH]` with a specific reason. A weak, forced angle is worse than an honest skip.

---

### TYPE 1: CONTRARIAN / COUNTERINTUITIVE

**Mandate:** Disagree with a widely-held assumption OR reveal that a commonly-applied practice produces the opposite of its intended result. The disagreement or inversion must be backed by specific evidence from the validated research.

**Prompt to self:**
- What does everyone in this exec's industry believe that the research suggests is wrong, oversimplified, or produces the opposite effect?
- The more widely-held the assumption, the more valuable the angle.

---

### TYPE 2: DATA-STORY

**Mandate:** Open with a specific statistic or data point from the NEWS_SCOUT findings. The data point is the engine of the angle — the narrative builds directly from what that number means.

**Prompt to self:**
- Which NEWS_SCOUT item has the most surprising or counterintuitive number?
- The hook IS the data. Don't bury it.

---

### TYPE 3: PERSONAL-STORY

**Mandate:** Experience-based. The angle puts the exec in the scene — something they observed, decided, got wrong, or had to learn. Grounded in the exec's documented role and domain.

**Prompt to self:**
- What situation would someone in this exec's role realistically have faced, given their pillars and the current research landscape?
- What would be a real moment of tension, failure, or realization — not a polished win story?

---

### TYPE 4: THOUGHT-LEADERSHIP

**Mandate:** Reveal a framework, mental model, or counterintuitive insight that helps the exec's audience see their work differently. Must be grounded in research.

**Prompt to self:**
- What pattern across the NEWS_SCOUT and ANGLE_HUNTER data suggests a framework the exec's audience hasn't articulated yet?

---

### TYPE 5: AUDIENCE-PAIN

**Mandate:** Name a specific frustration surfaced in the ANGLE_HUNTER community data. The exec's angle is a direct response to that documented frustration.

**Prompt to self:**
- What are practitioners actually complaining about in the ANGLE_HUNTER findings?
- Use the exact frustration language from the research as the hook signal.

---

### TYPE 6: PREDICTION

**Mandate:** Make a specific, defensible claim about where the exec's space is heading. "Defensible" means grounded in trend data from the research.

**Prompt to self:**
- What trajectory is visible in the NEWS_SCOUT and ANGLE_HUNTER data?
- Be specific: a prediction about "AI in general" is not a prediction.

---

### TYPE 7: COMMENTARY

**Mandate:** Tie directly to a specific current event or news item from the NEWS_SCOUT findings. This is the exec's take on something that just happened — not a timeless insight repurposed as commentary.

**Prompt to self:**
- Which NEWS_SCOUT fresh item is most relevant to the exec's audience?
- The news item is the anchor. The exec's expertise is the lens.

---

## Step 3 — Output Angle Tickets

Output each ticket in this format:

```
## ANGLE [N] — [TYPE]

**Title:** [5-8 words — specific and descriptive, not generic]
**Hook:** [The opening line of the post — 1 sentence]
**Core Message:** [What claim or insight does this post make? 1-2 sentences]
**Suggested Structure:** [e.g., "Problem → data point → exec's take → question to audience"]
**CTA Approach:** [1 specific question or invitation to comment]
**Evidence from research:** [Direct quote or close paraphrase from validated-research.md — cite which sub-agent it came from]

▶ Draft this: /draft [Exec Name] "[Title]"
```

If a type produced `[NO STRONG MATCH]`:

```
## ANGLE [N] — [TYPE]: NO STRONG MATCH

[Reason]
```

---

## Step 4 — Completion Message

```
[N] angle tickets generated above. Pick any and use:
  /draft [Exec Name] "[title]"
to draft the full post.

Validated research saved at: resources/sessions/[exec-name]/validated-research.md
```
