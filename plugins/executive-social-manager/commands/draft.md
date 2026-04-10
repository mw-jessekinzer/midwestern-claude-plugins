---
description: Draft a LinkedIn post for an exec on a given topic. Generates 2–3 quick angle options (no research pipeline), then drafts and runs full QA. Pauses at human review gate.
argument-hint: "<exec name> <topic>"
---

# Draft

Trigger: User types `/draft [exec name] [topic]` or says "draft LinkedIn for [exec] about [topic]" or any natural-language equivalent.

Hand off immediately to the `workflow-orchestrator` skill with the following context:

- **Mode:** draft
- **Exec name:** extracted from the command (everything before the topic)
- **Topic:** extracted from the command (everything after the exec name)
- **Run MODE 1** in the workflow-orchestrator: load minimal profile → generate 2–3 quick angle options → user picks (or says "just go") → draft → full QA pipeline → human review gate → formatted output

If the exec name matches multiple profiles or is ambiguous, ask the user to clarify before proceeding.

If no topic is provided, ask the user to provide one before proceeding.

**Key behavior:** This command does not run the 3-phase research pipeline. Angle options are generated from the exec's profile and Claude's knowledge. Any supporting data the draft includes is labeled `[ESTIMATED — not validated]` in the QA summary. To get research-grounded angles first, run `/research [exec]` instead.

**Small batch variant:** If the user provides multiple exec names separated by commas (max 3), run the full workflow for each exec sequentially, completing one before starting the next.
