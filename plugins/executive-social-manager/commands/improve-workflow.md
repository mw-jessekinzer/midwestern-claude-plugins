---
description: Voice calibration mode. Paste exec feedback or draft edits — the self-improver analyzes the pattern and proposes specific Voice DNA changes for approval.
argument-hint: "[optional: exec name to focus on]"
---

# Improve Workflow

Trigger: User types `/improve-workflow` or says "calibrate [exec]'s voice" or "the voice isn't landing right" or "I want to update the Voice DNA."

Hand off to the `self-improver` skill with the following context:

- **Mode:** voice calibration
- **Scope:** exec Voice DNA only
- **Output:** specific before/after Voice DNA changes proposed for user approval — never applied automatically

The self-improver will:
1. Ask the user to paste feedback or edits from a recent draft
2. Identify what the edits reveal about the exec's voice (gaps, over-documented traits, missing patterns)
3. Propose a specific change to the Voice DNA section of the exec's profile file
4. Wait for "apply" before providing the full updated profile content for copy-paste

Nothing is changed automatically. The user approves every modification.

Common use cases:
- After an exec reviews a draft and provides edits: `/improve-workflow` then paste their changes
- When a post voice keeps drifting in the same direction: `/improve-workflow [exec name]`
- After onboarding a new exec whose voice profile is thin: `/improve-workflow` to build it out from early drafts
