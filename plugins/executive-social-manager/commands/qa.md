---
description: Run the full 3-pass QA pipeline (fact-check, voice, anti-AI) on a draft you wrote yourself. Pauses at human review gate.
argument-hint: "<exec name>"
aliases: [/qa]
---

# QA

Trigger: User types `/qa [exec name]` or says "QA this draft for [exec]" or any natural-language equivalent.

Hand off immediately to the `workflow-orchestrator` skill with the following context:

- **Mode:** qa
- **Exec name:** extracted from the command
- **Run MODE 4** in the workflow-orchestrator: load full exec profile with Voice DNA upfront → prompt user to paste their draft → full QA pipeline (3 passes) → human review gate → formatted output

If the exec name matches multiple profiles or is ambiguous, ask the user to clarify before proceeding.

**Key behavior:** This is the only entry point that loads Voice DNA before drafting — because the draft already exists. No research, no angle generation. QA only.
