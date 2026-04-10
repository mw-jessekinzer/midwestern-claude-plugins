---
name: qa-pipeline
description: Automated 3-pass QA pipeline for LinkedIn post drafts. Runs before any human sees the draft. Pass 1 = fact-check (verify all claims, auto-fix). Pass 2 = Voice DNA rewrite (load full exec Voice DNA, rewrite to authentic voice). Pass 3 = delegates to the anti-ai-humanization skill (structural humanization, cognitive grounding, lexical verification). Outputs a compact QA summary + polished draft for human review.
---

# QA Pipeline

Runs three sequential passes on the structural draft before any human sees it. No human input between passes — this is fully automated.

**Input:** Structural draft from post-generator + full exec profile (used only in Pass 2)
**Output:** Single polished draft ready for human review

---

## PASS 1 — Fact Check

Verify every factual claim in the draft. Auto-fix or flag for removal. Update the draft silently — do not show the user intermediate states.

### What to Check

Scan for and verify every:
- Statistic or percentage ("83% of marketers...", "cold calls convert at 3x...")
- Named company, product, or event
- Date or timeline claim
- Causal claim ("X caused Y", "because of X, Y happened")
- Quote attributed to a specific person
- The exec's own company data or public claims

### Claim Verification Rules

**Statistics and data:**
- Trace to the original dataset or study — not the article citing it
- Verify the number, collection date, and population described
- Red flag: no named source in the draft

**Quotes:**
- Search exact text: `"[quote]" [speaker name]`
- Trace to original transcript, recording, or publication
- Red flag: paraphrased or undated quotes

**Causal claims:**
- Search for research or official analysis on the relationship
- Correlation without causal mechanism = Misleading

**Technology claims:**
- Use official documentation and specs
- Ignore press releases and marketing copy

**Source freshness:**
- Time-sensitive claims require sources from the last 6 months
- Sources older than 18 months for time-sensitive claims: downgrade to Unverifiable
- Evergreen facts (historical events, scientific constants): no freshness requirement

### Verdicts

| Verdict | When to use |
|---|---|
| ✅ True | Supported by credible, appropriately fresh sources |
| ❌ False | Directly contradicted by credible evidence |
| ⚠️ Misleading | Technically true but framed to create a false impression |
| ❓ Unverifiable | Cannot be confirmed — state why and what would confirm it |

### Auto-Fix Rules

- **❌ False claim:** Remove or replace with a verifiable alternative. If the claim is central to the post's argument and cannot be replaced, flag it for the human gate with a specific note.
- **⚠️ Misleading claim:** Reframe with the correct context or remove.
- **❓ Unverifiable stat:** Reframe as the exec's personal observation ("In my experience..." / "From what I've seen...") OR remove it. Do not leave unverifiable claims as factual assertions.
- **✅ True claim:** Leave unchanged.

### Pass 1 Output (internal — not shown to user yet)

Track what was changed for the final summary:
- List of claims verified ✅ (count only)
- List of changes made (before → after) for any ❌, ⚠️, or ❓ claims

---

## PASS 2 — Voice DNA Rewrite

**This is where Voice DNA loads for the first time.** Load the full Voice DNA section from the exec's profile: tone, signature moves, avoid list, and any real voice examples.

### What to Do

Rewrite the fact-checked draft to match the exec's authentic voice. This pass:
- Applies the exec's tone, signature moves, and structural habits
- Removes any generic phrasing that doesn't sound like this specific person
- Enforces the exec's avoid list (specific words, phrases, structural patterns)
- Does NOT search for new content — rewrites only what exists
- Does NOT change the core factual content from Pass 1

### Voice Application Rules

**Tone:** Apply the exec's documented tone from start to finish. Every sentence should sound like it came from this person — not from a generic professional.

**Signature moves:** Work in the exec's documented structural habits:
- How they open (counterintuitive claim, specific frustration, real observation)
- How they build an argument (their typical rhythm and pattern)
- How they close (their CTA style, ending question format)

**Avoid list:** Review every phrase against the exec's avoid list. Any flagged language — specific words, tone patterns, structural habits they don't use — must be removed and rewritten.

**Voice wins over structure.** If the exec's voice calls for something different than the draft's structure, adjust the structure. The voice profile is the authority.

### Pass 2 Output (internal)

The rewritten draft with Voice DNA applied. Track 2–3 notable voice changes for the summary.

---

## PASS 3 — Anti-AI Humanization

Run the full `anti-ai-humanization` skill on the voice-rewritten draft. All three sub-passes (Structural Humanization, Cognitive Grounding, Lexical Verification) run in sequence with no human input.

**Do not surface the skill's internal change log.** Collect only what's needed for the one-line summary in the final output (below).

For the summary line, track:
- **Structural fixes:** count of rhetorical theater removals + burstiness adjustments + transition chain removals
- **Vocab swaps:** count of banned words replaced
- **Knowledge boundary:** quote it verbatim if one was installed; omit if none
- **Read-aloud check:** ✅ or flag any remaining concern

### Pass 3 Output (internal)

The humanized draft + the four tracking items above. Nothing else surfaces.

---

## Final Output

After all three passes, output:

```
⚙️ QA COMPLETE — [Exec Name]

P1 FACT:   ✅ [N] claims verified · [N] fixed: [brief note, or "none"]
P2 VOICE:  [Exec] DNA applied · [change 1, max 10 words] · [change 2, max 10 words]
P3 HUMAN:  [N] structural · [N] vocab · boundary: "[quote]" · ✅

────────────────────

DRAFT:

[Full post text — post all three passes]

────────────────────
```

If no knowledge boundary was installed, omit that segment from the P3 line entirely.

The polished draft and QA summary are ready for human review.

---

## Hard Rules

- **Never skip a pass.** All three run in sequence every time, no exceptions.
- **Do not expose the anti-AI change log.** The full output from the anti-ai-humanization skill stays internal. Only the four summary items surface in the QA output line.
- **Never fabricate sources.** If a claim cannot be verified, reframe or remove. No invented URLs or studies.
- **Voice DNA does not load before Pass 2.** This is by design — the research and draft steps are intentionally voice-agnostic to keep early context lean.
- **Do not expose intermediate drafts.** The user sees only the final polished draft, not mid-pipeline versions.
- **Voice over template.** When the exec's Voice DNA calls for a structural deviation from post-generator defaults, the exec's voice wins.
- **Re-runs on specific passes only.** If the reviewer requests changes via comment syntax, only re-run the affected pass(es) — not the full pipeline.
