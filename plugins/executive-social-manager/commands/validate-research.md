---
description: Run Phase 2 validation on completed research for a specific exec
argument-hint: "[exec name]"
---

Run the **validator** skill for [exec].

Read `resources/sessions/[exec-name]/raw-research.md`, re-resolve the current date via Bash, then run FACT_CHECKER and ALIGNMENT_CHECKER sub-agents.

- On pass: write `resources/sessions/[exec-name]/validated-research.md` with `status: VALIDATION_PASSED`
- On fail: output a specific correction report — do not write validated-research.md

Exec: $ARGUMENTS
