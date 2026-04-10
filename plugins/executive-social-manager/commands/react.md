---
description: Draft an exec's reaction to a URL or pasted text. Full QA pipeline runs on draft claims (not the source). Pauses at human review gate.
argument-hint: "<exec name> <URL or pasted text>"
aliases: [/react]
---

# React

Trigger: User types `/react [exec name] [URL or pasted text]` or says "draft [exec]'s take on [source]" or any natural-language equivalent.

Hand off immediately to the `workflow-orchestrator` skill with the following context:

- **Mode:** react
- **Exec name:** extracted from the command (the name before the URL or pasted text)
- **Source:** the URL or pasted text following the exec name
- **Run MODE 3** in the workflow-orchestrator: ingest source → draft exec's reaction → full QA pipeline → human review gate → formatted output

If the exec name matches multiple profiles or is ambiguous, ask the user to clarify before proceeding.

If no source (URL or pasted text) is provided, ask the user to provide one before proceeding.

**Key behavior:** The source is inspiration, not a document to summarize. The exec's angle is primary. QA Pass 1 checks claims the *draft makes* — it does not validate the source itself.
