# Changelog

## 3.0.0 (2026-02-27)

Decomposed monolithic idea-researcher into a 3-phase research pipeline to prevent quality degradation from context window depth ("the dumb zone"). Each phase runs in a separate conversation turn with a fresh context window. File-based handoffs carry research between turns.

**New skills:**
- **researcher:** Phase 1. Resolves current date via Bash (`date +%Y-%m-%d`), computes WINDOW_30_DAYS and WINDOW_6_MONTHS dynamically. Runs three sub-agents: NEWS_SCOUT (5–8 news items with time gates), PERSONA_ANALYST (avoid list / hook patterns / angle gaps from exec profile), ANGLE_HUNTER (4–6 viral/controversial takes with engagement thresholds). Writes `resources/sessions/[exec-name]/raw-research.md`.
- **validator:** Phase 2. Re-resolves current date independently. Runs FACT_CHECKER (date recency, attribution specificity, verifiability — does not verify live URLs) and ALIGNMENT_CHECKER (recycling detection, contradiction detection). Pass → writes `validated-research.md`; Fail → outputs specific correction guidance, no file written.
- **angle-generator:** Phase 3. Reads validated-research.md and runs 8 pre-typed angle agents sequentially (CONTRARIAN, DATA-STORY, PERSONAL-STORY, THOUGHT-LEADERSHIP, AUDIENCE-PAIN, PREDICTION, COUNTERINTUITIVE, COMMENTARY). Each agent has a fixed type mandate and a `[NO STRONG MATCH]` escape hatch. Outputs 8 angle tickets with `/draft-for` commands.

**New commands:**
- `/research-for [exec]` — triggers Phase 1
- `/validate-research [exec]` — triggers Phase 2
- `/generate-angles [exec]` — triggers Phase 3

**Updated commands:**
- `/batch-ideas` — now redirects to the 3-step pipeline with usage instructions. No longer invokes the workflow-orchestrator directly.

**Deprecated skills:**
- `idea-researcher` — kept for reference, marked deprecated. Do not invoke. Replaced by researcher + validator + angle-generator.

**Infrastructure:**
- New per-exec session folders at `resources/sessions/[exec-name]/` — created on first research run, named using lowercase-hyphen convention matching exec profile filenames.
- Dynamic date resolution at Step 0 of each phase skill prevents LLM from anchoring date ranges to training data cutoffs.

**Unchanged:**
- `workflow-orchestrator`, `post-generator`, `qa-pipeline`, `self-improver` — no changes
- `/draft-for`, `/improve-workflow` — no changes
- All exec profiles — no changes

## 2.0.0 (2026-02-26)

Full plugin redesign. 7 steps instead of 11, 5 skills instead of 7, 3 commands instead of 4. No external integrations.

**New skills:**
- **qa-pipeline:** New combined skill running 3 sequential automated passes — fact-check, Voice DNA rewrite, anti-AI humanization — before any human sees the draft. Replaces the separate fact-checker and anti-ai-humanize skills. Voice DNA now loads for the first time at Pass 2 of this pipeline, not at Step 1 of the workflow.

**Rewritten skills:**
- **workflow-orchestrator:** Full rewrite. 7-step flow replaces the 11-step v1. Single human review gate at Step 6 (down from two gates at Steps 5 and 9). Minimal profile loading at Step 1 (name, role, audience, pillars — no Voice DNA). All Notion, Asana, and Slack steps removed.
- **idea-researcher:** Now operates on lightweight profile only (no Voice DNA). Strict 4–6 month recency gate on all research tracks. Each angle validated against recent LinkedIn engagement signals before being presented. Low-signal angles dropped, not surfaced.
- **self-improver:** Stripped to voice calibration only. Accepts exec feedback or draft edits → proposes specific Voice DNA diffs → waits for "apply" before updating profile. Removed changelog tracking, pattern analysis, and general workflow improvement scope.

**Renamed + rewritten skills:**
- **post-generator** (was linkedin-post-generator): Voice-agnostic structural drafting. Preserved post structure rules, hook library, style examples. Voice DNA no longer applied here — moved to qa-pipeline Pass 2.

**Deleted skills:**
- `fact-checker` — merged into qa-pipeline
- `anti-ai-humanize` — merged into qa-pipeline
- `visual-prompt-generator` — removed entirely

**Updated commands:**
- `/batch-ideas` (was `/weekly-batch`): Updated to reference 7-step workflow and new skill names. Ideation-only mode, Steps 1–3.
- `/draft-for`: Updated to reference 7-step workflow and single review gate.
- `/improve-workflow`: Scoped to voice calibration only.
- Deleted: `/weekly-batch`, `/update-ideas`

**Exec profiles:**
- Removed `## Notion Output`, `## Asana Tickets`, and `## Visual Style` sections from all 6 Format A profiles (jesse-kinzer, Austin-Daniel, Kahlie-Huff, Ryan-Doss, Cyril-Jones, Austin-Edison) and the ADD-YOUR-OTHER-9-HERE template.
- Format B profiles (Sonya-Mead, Jerrod-Tracy, Dom-Paulk, Scott-Blevins) unchanged — they didn't have these sections.
- All other profile content (Voice DNA, Content Pillars, Audience, Validated Audience Pain Points) preserved exactly as-is.

## 1.0.1 (2026-02-19)

Post-launch quality pass after loading all 10 exec profiles.

- **idea-researcher:** Removed Jesse-specific hardcoding. Skill is now fully profile-agnostic. Subreddit and news source selection is driven dynamically from the loaded exec's role and content pillars via a role→channel mapping table.
- **workflow-orchestrator:** Replaced static profile loading with case-insensitive matching against the `# VOICE PROFILE:` header line (filename as fallback). Added explicit disambiguation block for the two Austin profiles (Austin Daniel / Austin Edison). Added `ADD-YOUR-OTHER-9-HERE.md` exclusion guard. Added Voice DNA inference logic for PART 2 format profiles (synthesized from exec role, domain, and audience) — surfaced at Step 1 with an `adjust voice:` escape hatch for Jesse to correct before drafting.
- **linkedin-post-generator:** Executive Roster section redirected from internal placeholder to `resources/exec-profiles/`. Step 1 updated to accept orchestrator-loaded profile context rather than managing its own exec list.
- **visual-prompt-generator:** Removed Jesse-specific "Calibration Notes" section. Replaced with a role→visual style mapping table covering all exec roles in the current roster (CGO, COO, Design Director, Director of AI, CTO, Marketing).
- **jesse-kinzer.md:** Fixed profile header from `# EXEC PROFILE:` to `# VOICE PROFILE:` to match the format expected by the orchestrator's name-matching logic.

## 1.0.0 (2026-02-19)

Initial release.

- Full 11-step orchestration workflow with mandatory human gates at steps 5 and 9
- Multi-executive support: up to 10 exec profiles, referenced by first name
- Bundled skills: linkedin-post-generator, idea-researcher, fact-checker, anti-ai-humanize, self-improver, visual-prompt-generator
- Live MCP wiring: Notion (idea logging), Asana (review tickets), Slack (draft package, never auto-send)
- Jesse Kinzer exec profile included as reference format
- ADD-YOUR-OTHER-9-HERE.md template for remaining executives
- Voice & Tone Examples resource with real LinkedIn post examples and post styles to avoid
