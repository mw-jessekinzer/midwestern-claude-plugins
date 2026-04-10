---
description: Start Phase 1 of the research pipeline for a specific exec. Runs NEWS_SCOUT, PERSONA_ANALYST, and ANGLE_HUNTER. Writes findings to resources/sessions/[exec]/raw-research.md.
argument-hint: "<exec name>"
---

# Research

Trigger: User types `/research [exec name]` or says "run research for [exec]" or any natural-language equivalent.

Run the **researcher** skill for [exec].

Load their profile from `resources/exec-profiles/`, resolve the current date via Bash, then run NEWS_SCOUT, PERSONA_ANALYST, and ANGLE_HUNTER sub-agents. Write all findings to `resources/sessions/[exec-name]/raw-research.md`.

Exec: $ARGUMENTS

**Next steps after this command:**
1. `/validate-research [exec]` — Phase 2: validate and score angles
2. `/generate-angles [exec]` — Phase 3: produce 8 angle tickets
3. `/draft [exec] "[angle title]"` — draft a post from a validated angle
